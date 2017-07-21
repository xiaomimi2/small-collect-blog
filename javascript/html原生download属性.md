###神奇的html/html5中的download（阅读笔记）

简单下载
```
<a href="a.jpg" download>下载</a>
```

可以指定下载文件名
```
<a href="index_logo.gif" download="_5332_.gif">下载</a>
```


这个属性存在兼容性问题。不过比较新的浏览器基本都支持。


###补充杂记
在XMLHttpRequest Level2的背景下。我们ajax可以发送任DOMString,Document,FormData,Blob,FIle,ArrayBuffer这些类型。

1.DOMString类型  UTF-16字符串,javascript就是用了这种编码的字符串。所以就相当于普通字符串。

2.Document类型
可以近似堪称XML数据类型

3.FormData对象
可以模拟键值对表单数据。最重要的是可以艺术部上传/*二进制文件*/
`new Formdata(HTMLFormElement)`
有append方法。append(DOMString键, Blob值,DOMString文件名[可选])
append(DOMString键,DOMString值)

4.Blob数据对象
window.URL.createObjectURL(blob) //兼容性要注意
window.URL.revokeObjectURL(blob)
Blob的属性size,表示字节数**只读**
Blob的属性type,表示MIME类型,如果类型未知,返回空字符串**只读**。
Blob的方法slice(start, end, contentType)都是可选
`var myBlob = new Blob(arrayBuffer)`


5.File对象 从属于Blob对象
属性
File.lastModifiedDate
File.name 文件名称
Blob.size属性文件大小
Blob.type 文件的MIME类
方法 
二进制形式返回文件数据  FileReader.readAsBinaryString()
data:URL编码字符串数据  FileReader.readAsDataURL()
返回给定字符串编码方式的的文本  FileReader.readAsText()方法

6.ArrayBUffer对象 表示二进制数据的原始缓存区,该缓存区用于存储各种类型化数组的数据
长度固定

###基于download属性的一段小文本下载
```javascript
var eleTextArea = document.querySelector('textarea')
var eleButton = document.querySelector('input[type="button"]')
var funDownload = function (content, filename){
    var elelink = document.createElement('a')
    elelink.download = filename
    elelink.style.display='none'
    var blob = new Blob([content])
    document.body.appendChild(elelink)
    elelink.click()
    document.body.removeChild(elelink)
}
if('download' in document.createElement('a')) {
    eleButton.addEventListener('click', function (){
        funDownload(eleTextArea.value,'test.html')
    })
} else {
    eleButton.onclick = function () {
        alert('浏览器不支持')
    }
}