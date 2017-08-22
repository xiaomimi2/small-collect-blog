###babel
babel-cli 命令行转码
`bable script.js` 只能进行简单的ES6语法的翻译

bebel-core
如果某些代码需要使用Babel的API进行转码，就需要使用babel-core模块。这是专门为了转码使用的。一般开发不需要


babel-plugin-transform-runtime  为了使用某个工具函数，你需要在每个文件都引用一次。这种重复是没有必要的。所以这个插件可以引用babel-runtime来避免编译时重复。另外会创建一个沙盒环境，将


babel-polyfill可以支持ES6新的函数对象方法，但是是全局的。