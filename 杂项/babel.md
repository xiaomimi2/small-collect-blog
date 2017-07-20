##babel的使用笔记

1. babel默认只转换新的javascript语法(syntax),不转换新的api,比如iterator,generator,set,map,proxy,reflect,symbol,promise等全局对象。比如一些定义在全局对象上的方法，比如object.assign都不会转码。这个时候就要使用babel-polyfill，为当前环境提供一个垫片。 
具体babel不支持编译的可以看[这里](https://github.com/babel/babel/blob/master/packages/babel-plugin-transform-runtime/src/definitions.js)代码,需要babel-polyfill。


