###this
1.函数中的this不是指向函数本身。具名函数指向自身可以调用函数名，匿名函数可以使用arguments.callee的方法指向自身。arguments.callee已被弃用，最好能够避免使用匿名函数。

2.this不是一定指向函数的作用域。但是需要明确的是this在任何情况下都不指向函数的词法作用域。
```javascript
   function foo() {
     var a = 1
     this.bar()
   }
   function bar（） {
      console.log(this.a)
   }

   foo() //a is not defined
```
this的帮i的那个和函数声明的位置没有任何关系，只取决于函数的调用方式。在一个函数被调用时，会创建一个执行上下文对象，包括函数在哪里被调用（调用栈）,函数的调用方法，传入的参数等信息。


3.根据调用位置判断this,最重要的就是分析调用栈，得到真正的调用位置。然后根据如下四条规则来判断

默认绑定
:    如果使用严格模式，全局对象是无法默认绑定的，否则就是绑定到windows对象上
隐式绑定
:    函数无论是作为引用添加入对象还是直接在某个对象中定义，严格来说都不属于这个对象。然而调用位置会使用这个对象的上下文来调用它。当函数引用有**上下文对象**,隐式绑定规则会把这个this绑定到这个上下文对象。
```javascript   
function foo() {
    console.log(this.a)
}
var obj = {
    a: 42,
    foo: foo
}
var obj1 = {
    a: 2,
    obj2:obj2
}
obj1.obj2.foo()//42  先找到调用位置是在obj内。然后上下文环境是obj.所以最后a的结果是2
```

隐式丢失
:    也就是会默认绑定到全局对象或者undefined上，却决于是否有严格模式。
```javascript
function foo() {
    console.log(this.a)
}

function doFn(Fn) {
    Fn()
}
var obj = {
    a:2,
    foo: foo
}

var a = 'opps,global'
doFn(obj.foo) //‘opps,global’  参数传递就是隐式赋值。
```

显示绑定
:    call()和apply()
```javascript   
//简单的辅助绑定函数,这是一种硬绑定的方式
function bind(fn, obj) {
    return function () {
        return fn.apply(obj, arguments)
    }
}
//或者直接使用ES5中提供的内置的方法FUnction.prototype.bind
var bar = foo.bind(obj)

//很多第三方库函数和内置函数都提供一个可选的上下文context参数
[1, 2, 3].forEach(foo,obj) //这样把this绑定到obj
```
**forEach,filter等数组的原生方式还可以绑定this对象***

new绑定
:    进行了new操作后会创建一个新的对象，这个新对象会被执行[[原型]]连接。这个新对象会绑定到函数调用的this。*如果这个功函数灭有返回其他对象*，那么new表达式中的函数调用会返回这个新对象。

4.被忽略的this
如果你把null或者undefined作为this的绑定对象传入call,apply或者bind,这些值调用时会被忽略。实际应用的是默认绑定的规则。

5.硬绑定和软绑定
硬绑定this定死。软绑定还可以允许它进行隐式绑定。应用更广。
```javascript
if(!Function.prototype.softBind) {
    Function.prototype.softBind = function () {
        var arg = Array.prototype.slice.apply(arguments,1)
        var fn = this
        var bound = function () {
            return fn.apply( !this || this === (window || global) ? obj : this,
            args.concat(arguments)
        }
        bound.prototype = object.create(fn.prototype)
        return bound
    }
}


