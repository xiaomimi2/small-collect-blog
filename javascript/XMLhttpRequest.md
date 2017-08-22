##XMLHttpRequest 新的属性介绍。

###XMLHttpRequest level 1.0
1.不能发送跨域请求
2.不能发送二进制文件，如图片，视频，音频等。只能发送纯文本数据
3.在发送和获取数据的过程中，无法实时获取进度信息。只能判断是否完成

###XMLHttpRequest level 2.0
1 在服务器允许的情况下允许跨域 
2 支持发送和接受二进制数据
3 新增了formData对象来管理表单数据
4. 发送和获取数据时可以获取进度信息
5. 可以设置http请求的时限 xhr.timeout= 3000  xhr.ontimeout 事件，用来指定回调函数



###兼容性问题
ie8/9,opera mini不支持xhr
ie10.ie11部分支持，不支持xhr.responseType为json
部分浏览器不支持timeout
部分浏览器不支持 xhr.responseType未blob


###发送a请求头部信息，比如content-Type,connection,cookie,accept-xxx等，xhr提供了setRequestHeader。
注意点:该方法的第一个参数不区分大小写。该方法必须写在open之后，send之前。这个方法可以调用多次，采用的是追加的方式
```
var client = new XMLHttpRequest()
client.open('GET','demo.php')
client.setRequestHeader('x-Test','one')
client.setRequestHeader('x-Test','two')
client.send()
```

###获取response header方法,只能返回特定的一些heaer字段
1.getAllResponseHeaders()
2.getResponseHeader()


###设置response的数据类型
1.老方法 xhr.overrideMimeType
2.新方法 xhr.responseType 属性.可以设置的数据格式的值有""(默认值，在不设置xhr.responseType的时候),"text","document"(希望返回xml格式的数据),"json","blob","arrayBuffer"

```
    老方法
    xhr.overrideMimeType('text/plain;charset=x-user-defined')
    var binstr = xhr.responseText
    for(var i = 0 ; len = binstr.length; i++) {
        var c = binstr.charCodeAt(i)
        var byte = c & 0xff
    }
    新方法
    var xhr = new XMLHttpRequest()
    xhr.open('GET','/home')
    xhr.responseType = 'blob'
    var blob = new Blob([xhr.response],{type:'image/png'})
```


###获取response的数据
1.xhr.response 
2.xhr.responseXML
3.xhr.responseText


###xhr.readyState，每次改变都会触发xhr.onreadystatechange事件
0   UNSENT  open方法未调用
1   OPENED  open方法已调用，send方法还没被调用
2   HEADERS_RECEIVED  send()方法已经被调用，响应头和响应状态已经返回
3   LOADING  正在下载响应体
4   DONE    整个数据传输过程结束，*不管本次请求结束或者失败*。

###xhr如果发送同步请求。open方法的第三个参数是布尔值。false就是同步，true是异步
注意 xhr.timeout = 0
    xhr.withCredentials 必须是false
    xhr.responseType 必须是 ""
待测试进度事件不能获取

### 获取上传下载进度
xhr.onprogress  //下载触发
xhr.upload.onprogress  //上传触发

###可发送的数据类型
ArrayBuffer
Blob
Document
DOMString
FormData
null

这些设置会影响conent-type的默认值
注意xhr.send()方法在断网的情况下会报错xhr.send(data)方法，则会抛错：所以需要用try-catch来捕捉错误。
```
try {
    xhr.send()
}catch(e) {
    //dosomething
}
```

###CORS跨域和xhr.withCredentials
在CORS标准中做了规定，默认情况下浏览器在发送跨域请求时不经发送任何认证信息(credentials),如cookie和http authentication schems,除非xhr.withCredentials未true。默认值是false

所以在跨域请求中，client端要手动设置xhr.withCredentials = true。且server端也必须允许request能够携带认证信息。（即response header 中包含 Access-control-Allow-Credentials:true）

**注意如果一旦跨域request能够携带认证信息，server端一定不能将Access-Control-Allow-Origin 为*，而是请求页面的域名**


###xhr和xhr.upload都有的事件 
xhr.onloadstart  xhr.send方法后立即触发，send方法不调用此方法不会执行
xhr.upload.onprogress xhr.send()之后，xhr.readyState=2之前,每50ms触发一次
xhr.onprogress  下载阶段，在xhr.readyState = 3时触发。每50ms触发一次
xhr.onabort 调用xhr.abort()后触发。
xhr.ontimeout 由onloadstart开始算到xhr.onloadend
xhr.onerror networrk error。网络错误触发。先是xhr.upload.onerror然后是xhr.onerror.如果上传已经结束，只有xhr.onerror会调用。
xhr.onload xhr.readyState = 4
xhr.onloadend 请求结束时触发
xhr.onreadystatechange事件是xhr特有的

###xhr事件触发顺序
一旦发生abort或timeout或error异常，先立即中止当前请求
将 readystate 置为4，并触发 xhr.onreadystatechange事件
如果上传阶段还没有结束，则依次触发以下事件：
xhr.upload.onprogress
xhr.upload.[onabort或ontimeout或onerror]
xhr.upload.onloadend
触发 xhr.onprogress事件
触发 xhr.[onabort或ontimeout或onerror]事件
触发xhr.onloadend 事件