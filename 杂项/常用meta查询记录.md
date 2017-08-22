##meta标签查询记录
meta标签总共两个属性.name属性主要用于描述网页。比如网页的关键字。与之对应的属性值是content，content中的内容是对name填入类型的具体描述。便于搜索引擎抓取。

###name属性值
1 keywords 网页关键字
`<meta name="keywords" content="博客,文科生">`

2 description 网页内容的描述
`<meta name="description" content="这是我的工作中经常需要查询使用的博客。用作记录">`

3 viewport 移动端窗口
`<meta name="viewport" content="width=device-width,initial-scale=1">`

4 robots 搜索引擎爬虫的索引方式。哪些页面不需要索引.content的参数有all，none(搜索引擎忽略此页),index,noindex(搜索引擎忽略此页),follow,nofollow。默认是all
(搜索引擎忽略此页)

5 author 标注网页作者

6 copyright 表明版权

7 renderer 双核浏览器渲染方式
```html
    <meta name="renderer" content="webkit">
    <meta name="renderer" content="ie-comp"> //ie兼容方式
    <meta name="renderer" content="ie-stand"> //ie标准模式
```


###http-equiv属性。相当于http的文件头作用，可以定义某些http参数
`<meta http-equiv="参数"  content="具体的描述"`

1.content-Type 设定网页字符集
`<meta http-equiv="content-Type" content="text/html;charset=utf-8">`
新版推荐写法 <meta chartset="utf-8">

2 X-UA-Compatible 浏览器采取何种版本渲染当前页面
`<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">//最新版渲染` 

3 cache-control制定请求和相应遵循的缓存机制
`<meta http-equiv="cache-control" content="no-cache">`
no-cache 先发送请求，与服务器确认资源是否被更改。如果未被更改，则使用缓存
no-store 不允许缓存，每次都要去服务器（安全措施）
public 缓存所有响应,但并非必须。
private 只为单个用户缓存，因此不允许任何中继进行缓存（CDN不允许缓存private的响应）
maxage max-age=60，表示可以缓存和重用60s
**no-siteapp**禁止当前页面在移动端浏览时，被百度自动转码。

4.expires 网页到期时间
`<meta http-equiv="expires" content="Sunday 26 October 2016 01:00 GMT" />`

5.refresh自动刷新并指向设定的网页
<meta http-equiv="refresh" content="2；URL=http://www.lxxyx.win/"> //意思是2秒后跳转向我的博客
