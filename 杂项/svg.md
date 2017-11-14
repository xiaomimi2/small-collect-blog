###学习SVG

####使用方式
1. 浏览器直接打开
2. img标签引用
3. html中直接使用svg标签
4. 作为css背景


####svg基本图像
`<rect>` x,y,width,height,rx, ry
`<circle>` cx(圆心),cy,r
`<ellipse>`椭圆 cx,cy,rx,ry
`<line>` x1,y1,x2,y2
`<polyline>`折线 多个点表示
`<polygon> ` 同上，但是会把最后一个点和第一个点连起来

####svg基本属性
fill,stroke,stroke-width,transform


####svg基本API
1.创建图形
`document.createElementNs(ns, tagName}`
2.添加图形
`element.appendChild(childElement)`
3.设置/获取属性
`element.setAttribute(name,value)`
`element.getAttribute(name)`
