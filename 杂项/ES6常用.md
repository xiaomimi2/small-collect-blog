##常用的ES6语法
[原帖地址](http://www.jianshu.com/p/ebfeb687eb70)
####let 和 var 的不同
```javascript
var a = [];
for (var i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 10
```


```javascript   
var a = [];
for (let i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 6
```

#### const
const 一旦定义后面修改会报错。经常用到的场景类似于
```javascript
const express = require('express')
```



####箭头函数
箭头函数可以让this一直是当前词法作用域
```javascript
function (i) { return i + 1 }
(i) => i + 1
```


```javascript   
function (x, y) {
    x++;
    y--;
    return x + y
}

(x, y) => {x++; y--; return x + y)}
```

#### template string``
```javascript
$('#result').append(`There are <b>${basket.count}</b> items in your basket,<em>basket.onSale</em> are on sale`) 
```

#### 解构
key值就是定义的变量名。value就是变量值
```javascript
let cat = 'ken'
llet dog = 'lili'
let zoo = {cat, dog}
console.log(zoo)) // object {cat: "ken", dog: "lili"}
```


####default
在以前我们想设置一个默认值
```javascript   
function animal(type) {
    type = type || 'cat'
    console.log(type)
}
```

使用了es6可以这样写
```javascript
function animal(type = 'cat') {
    console.log(type)
}
```

#### rest语法，拓展运算符（rest语法的反运算）
```javascript  
function animal(...types) {
    console.log(types)
}
animal('cat','dog','fish') //['cat','dog','fish']
```

####结构赋值的应用
1. 两个变量值的对换
`[a, b] = [b, a]`

2. 从函数返回多个值
`var [x, y, x] = message()` 

3. 返回一个对象
`var {name, age, wechatID} = message()`

4. 函数参数的运用
```javascript
function ordervalue(arr) {
    console.log(...arr)
}
ordervalue([1,2,3,4])
functio Disordervalue({a,b,c,d}) {
    console.log("name", a)
    console.log(b)
    console.log(c)
}
Disordervalue({a:'传输',b:"itclan",c:'xii'})
```

5. 提取json数据
```
 var jsonData = {
    id: 133444,
    sex: 'boy'
}
var {id, sex} = jsonData
```



####class,extends,super
```
class Animal {
    constructor () {//这个定义的是对象自己的。外面的其他方法都是可以共享的。
        this.type = 'animal'
    }
    says(say) {
        console.log(this.type + say)
    }
}

let animal = new Animal()
animal.says(hello) //animal hello

class Cat extends Animal{//class之间可以通过extends关键字实现继承
    constructor () {
        super()//必须在这里调用这个方法。因为实例没有自己的this对象，而是继承父类的this对象。然后对其进行加工。
        this.type = 'cat'
    }
}

let cat = new Cat()
cat.says('hello')//cat hello
```

####generator
生成器有两个最主要的应用
1. 实现迭代功能
2. 封装异步函数调用

