###pushState和replaceState,操作浏览器历史栈的方法
这个要注意浏览器的兼容性
```javascript  
history.pushState(data,null, url)
//data 表示浏览器的一个对象
//null title值，一般直接定为null
//string 用以改变当前的url值。注意这个不能是跨域
```

pushState会增加历史记录。replaceState只会更改当前的url.历史记录改成这个url

如果不发生浏览器刷新的话，再使用history.back()等事件会触发popState事件
```javascript  
history.pushState({page: 1}, null, '?page=1');
    history.pushState({page: 2}, null, '?page=2');

    history.back(); //浏览器后退

    window.addEventListener('popstate', function(e) {
        //在popstate事件触发后,事件对象event保存了当前浏览器历史记录的状态.
        //e.state保存了pushState添加的state的引用
        console.log(e.state);  //输出 {page: 1}
    });
```

###应用在单页应用的路由中,简单的小型路由代码
```html
 <a href="/post"></a>
 <a href="/login"></a>
```

```javascript
const Router = []
const addRoute = (path = '', handler = () => {}) => {
    let obj = {
        path,
        handler
    }
    Router.push(obj)
}

addRouter('/post', function () {})
addRouter('/login', function (){})

const routerHandler = (path) => {
    Router.forEach(function (item, index) {
        if(item.path == path) {
            item.handler.apply(null, [path])
            return true
        }
    })
    return false 
}
/*阻止a标签的默认事件*/
document.addEventListener('click', function (e) {
    var dataset = e.target.dataset
    if(dataset) {
        if(routerHandler(dataset.href) {
            e.preventDefault()
        })
    }
})
···
