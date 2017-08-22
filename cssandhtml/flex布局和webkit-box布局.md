##flex布局和webkitbox布局

###容器的属性
1.flex-direction:确定主轴的方向 row(水平)/row-reverse/column(竖直,起点在上端)/column-reverse
2.flex-wrap:定义如果一条轴线不够如何换行 nowrap/warp/warp-reverse(第一行会在下方)
3.flex-flow 是上面两个属性的缩写形式，默认值是row nowrap
4.justify-content 在项目主轴上的对应方式 flex-start/flex-end/center/space-between/space-around
5.align-items 项目在交叉轴上的对其方式 flex-start/flex-end/center/baseline/stretch
6.align-content 定义了多根折线的对齐方式.如果项目只有一根轴线，那个这个属性不起作用。flex-start/flex-end/center/stretch/space-around/space-between

### 项目的属性
1.order 定义项目的排列顺序，数值越小越前。默认是0
2.flex-grow 项目的放大比例。默认为0.即使存在剩余空间，也不放大。如果所有的项目都是1,那么会**等分**剩余空间。
3.flex-shrink 定义了项目的默认缩放比例。默认是1.即如果空间不足，该项目将缩小。如果一个项目的flex-shrink属性为0,其他项目的属性都为1.那个其他项目将缩小。他不变
4.flex-basis 定义了在分配多余空间之前，项目占据的主轴空间。用来季孙主轴是否有剩余空间。默认值是auto.   `<length>/auto`.设置了固定值后，项目会占据固定空间。
5.flex属性是flex-grow, flex-shrink,flex-basis三个属性的简写。默认是0 1 auto。具有两个快捷值 auto(1,1,auto)可放大，none(0,0,auto) 可缩
6.align-self 允许单个项目有和其他项目不一样的对齐方式。默认值是auto，继承父元素的align-items属性。如果没有父元素，等同于stretch



