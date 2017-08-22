##lodash源码阅读小收获
###Array数组的方法实现

1.chunk 方法,均分数据。使用了slice(index, index+= size) 2.compact方法,除去数组中假数据包括NaN,false,0,"",undefined,false
`if(value)`即可去除所有无法===true的值。
3.difference方法.
https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol/isConcatSpreadable
数组默认在进行concat的时候是先先展开的,类数组对象默认是不展开的。
```javascript
var fakeArray = { 
  [Symbol.isConcatSpreadable]: true, //类数组对象加上这个属性就可以展开了。
  length: 2, 
  0: "hello", 
  1: "world" 
}
```
Symbol.isConcatSpreadable直接代表了三个属性,writable,enumerable,configurable。

```javascript
 const { length } = array //这样可以直接取得数组的长度
```


