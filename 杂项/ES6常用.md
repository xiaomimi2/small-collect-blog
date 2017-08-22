##常用的ES6语法

###历史
ES5版本引入Object.create(),Object.defineProperty(),getters,setters，严格模式以及JSON对象主要是JSON.stringify,JSON.parse方法,还有数组方法map(),filter()这些。
[原帖地址](http://www.jianshu.com/p/ebfeb687eb70)

###let 和 var 的不同
1.let不存在变量提升
2.let在for循环中循环变量和函数内部的变量i不在同一个作用域
```javascript
    for(let i = 0 ; i < 5; i++) {
        let i = 'aaaa' //这里两个i相互没有关联
    }
    console.log(i) //Uncaught ReferenceError: i is not defined

```
3.*暂时死区*区块中如果存在let和const,这些区块凡是在声明前使用这些变量，就会报错
。即使外面有全局变量。
4.不允许重复定义,和var混用也是重复定义
5.函数声明请不要放在块级作用域中，因为es6的浏览器和其他浏览器的实现效果不相同。无法预测。


### const
const不能光定义不赋值，必须一开始就初始化。另外是也只在声明的块级作用域内生效，也不会变量提升，也存在暂时性死区，也不可重复声明。 
**注意** const指向的变量如果是一个常量，就保存这个值，如果是一个对象，那么就只是保存指向这个对象的指针。对象自身仍然可以发生改变
```javascript
    const obj = {}
    obj.name = 'test' //不会报错
```

const 一旦定义后面修改会报错。经常用到的场景类似于
```javascript
const express = require('express')
```


####es6的声明方式和之前的差别
之前对window对象的属性赋值和直接全局变量的赋值是同一件事。为了改变这个设计，新的let,const,class命令声明的全局变量不再属于顶层对象的属性



###解构赋值
1.数组
只要某种数据具有iterator接口，就可以采用数组形式的解构赋值。（generator函数具有iterator接口）
```javascipt
 let [foo,[[bar], baz]] = [1,[[2],3]]
 let [x,y,...z] = ['a']

 //允许指定默认值
 let [x,y='b'] = ['a', undefined] //判断一个位置的赋值是不是全等于undefined,默认值才能生效
 let[x,y='b'] = ['a', null]
 //只有赋值是undefined的时候f函数才会进行计算。这个是惰性求值的。
 function f() { console.log('aaa')}
 let [x = f()] = [1]
```

2.对象
对象有一个要求就是变量必须与属性同名，才能取到正确的值。
对象结构也存在默认值。
```javascript
let {bar, foo} = {foo:'aaa', bar:'bbb'}
bar// 'bbb'

let {first:f, last:l} = {fist:'hello', last:'world'}
f//'hello
l//'world'

//与数组一样可以用于嵌套结构的对象
let obj = {
    p: [
        'hello', {
            y: 'world'
        }
    ]
}
let {p:[x,{y}]} = obj
//这里p不会被赋值，因为不是变量
let{p,p:[x,{y}]} = obj
p//['hello',{y: "world"}]
```

**注意已声明变量的解构赋值**
```javascript
    let x ;
    {x} = {x : 1} //报错 因为会将{x}理解成一个代码块
    //正确的写法是
    ({x} = {x : 1})
```


3.字符串的解构赋值
```javascript
    let {length:len} = 'hello'
    len // 5
```

4.数值和布尔值的解构赋值
```javascript
let {toString: s} = true
s === Boolean.prototype.toString //true
```

5.函数参数的解构赋值
```javascript
    function add([x,y]) {
        return x + y
    }
    add([1,2])
```

函数也可以使用默认值
```javascript  
    function move({x=0, y = 0} = {}) {
        return [x, y]
    }
    move({x:8,y:3}) //[3, 8]
    move({x: 3}) //[3, 0]
    move({}) //[0,0]
    move() // [0,0]
```


6.注意圆括号问题
- 变量声明不使用
- 函数参数
- 赋值语句的模式
- 总结就是不要妨碍key值即可


###字符串的赋值
*javascript允许以\uxxxx形式表示的字符,其中xxxx表示unicode码点,但是只能表示\u0000到\uFFFF之间的数*

Javascript默认是utf-16编码


###数组的扩展
1.扩展运算符的使用。

- 好比rest参数的逆运算 `console.log(...[1,2,3])`
- 可以用于多参数传参。可以直接合并数组，不需要调用concat方法`[1,2,...more]`
- 当把扩展运算符用于数组赋值，只能放在参数的最后一位`[first,...last] = ['foo']`
- 还可以将字符串转为真正的数组`[...'hello']`
- 注意javascript会将32位的unicode字符，识别成两个字符。使用扩展运算符可以避免这个问题
- 存在iterator接口的对象都可以用扩展运算符转换未真正的数组


```
'x\uD83D\uDE80y'.length // 4
[...'x\uD83D\uDE80y'].length // 3
```

2.Array.from()用于将两类对象变成真正的数组，分别是类数组对象和具有iterator接口的可遍历的对象。还可以接受第二个参数，类似与数组的map方法，对每个元素进行处理

*扩展运算符背后调用的是遍历器接口iterator,array.from则还支持类数组对像。本质特征是只要有length属性。*

3.Array.of()方法哟关于将一组值转换为数组，主要是行为同意。因为Array方法如果不传参数默认就是一个空数组，一个参数只是定义了数组的个数。这里是传几个参数就是数组的元素。

4.find(),findIndex(),参数是回调函数。可以接受三个参数value(当前值),index(当前位置),arr(原数组).findIndex()和indexof方法类似。但是针对NaN的判断可以使用Object.is的方法来判断

5 fill()接受三个参数，要填充的值，起始位置，结束位置（不包括）.

6 数组实例的entries

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


####for of
*与for in的差别*for in 循环的缺陷: 除了便利数组元素，还会遍历自定义属性。在某些请款下可能按照随机顺序遍历。这个只适合用在普通对象上。
*与forEach的差别*可以用来遍历集合对象，数组和类数组对象，还可以遍历字符串。这个方法与forEach方法相比的好处是可以正确响应break,continue,return语句