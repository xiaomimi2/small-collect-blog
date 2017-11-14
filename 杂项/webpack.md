##webpack教程摘录

###安装
npm install webpack 
npm install webpack-dev-server

###主要用途
1.将依赖树拆分成按需加载的模块
2.初始化加载的耗时尽量少
3.各种静态资源可以看作模块
4.将第三方库整合成模块的能力
5.可以自定义打包逻辑
6.适合大醒目，无论单页还是多页

###webpack常用构建命令
```javascript
webpack --config webpack.min.js //另一份配置文件
webpack --display-error-details //显示异常信息
webpack --watch //监听变动自动打包
webpack -p //压缩混淆脚本
webpack -d //生成map映射文件，，告知哪些模块最终被打包的地方
```

###webpack-dev-server
1.iframe模式下，无需额外配置
http://localhost:8080/webpack-dev-server/index.html
2.inline模式
```javascript
webpack-dev-server --inline//直接访问http://localhost:8080

//如果是node.js API的方法，需要手动webpack-dev-server/client?http://localhost:8080加到配置文件的入口文件处。
//
```


###webpack配置项详解
####entry,output模块
可以是字符串，代表一个文件，多个文件可以用数组，也可以是对象
```javascript
entry: {
    page1:'./page1.js',//这里的page1会变成chuckname
    page2:'./page2.js',
    vendors:'./src/vendors'
}
output:{
    path:__dirname,//这是绝对路径
    filename:"[name].bundle.js",
    chuckFilename:"[id].bundle.js",//非入口的chunk文件名
    publicPath:'assets',//用于script的引用路径，
    crossOriginLoading:false,//false/anonymous(发送不带凭据的跨域加载)/use-credentials(发送带凭据的跨域请求)
    devtoolLineToLine:false,//映射sourcemap让生成资源的每一行都映射到原始资源的同一行。可用于性能优化。
    //{test,include,exclude}对象，可以指定特定对象
    hotUpdateChunkFilename:[id].[hash].hot-update.js,//热更新chunk文件名
    hotUpdateFunction:'webpackHotUpdate',//用于异步加载热更新chunk的jsonp函数。
    hotUpdateMainFilename:[hash].hot-update.json,//[hash]被compilation生命周期的hash替换。
    jsonpFunction:'webpackJsonp',//webpack中用于异步加载chuck的jsonp函数   
    libraryTarget:'var',//'var' ,'this','commonjs'(exports['library'] = xxx),'commonjs2(module.exports=xxx)','amd'(到处为AMD，通过library选型设置名称),'umd'（导出为AMD,commonjs2,或者为root的属性）
    library：??,//！！！,
    sourceMapFilename:[file].map//file是js文件的文件名，chunk的id,compilateion的hash

}   

```
####开启source-map
config中添加 devtool:'eval-source-map'

####module模块  配置loader
module:{
    rules:[{
        test:/\.css$/,
        use:[{
            loader:'style-loader'
        },{
            loader:'css-loader',
            options:{
                modules:true
    }
        }]
    }]
}
####plugin模块 
```javascript
plugins:[
    new webpak.optimize.UglifyJsPlugin(),
    new HtmlWebpackPlugin({template:'./src/index.html'})
]





```

###文件分离，按需加载
```javascript
require.ensure(['jquery','imgScroll'],function (require) {
    var $ = require('jquery')
    require('imgScroll')

})
```

###搜索路径变量
```javascript   
var path = require('path')
module.exports = {
    resolve:{
        alias:path.join(__dirname,'./app/src/scripts')
    }
}

//这样后,获得./app/src/scripts/main.js，可以直接使用require('js/main.js')
```


