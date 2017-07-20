###less语法记录


1. 定义变量
用@表示。比如变量a是@a
less变量可以进行加减乘除运算
```
@color:#000
.box{
    color:@color+111
}
//输出结果是#111
```
妙用less变量,当作class名使用。后者在作为字符串中的一部分。
```
@bg:box
.@{bg}{}  //输出.box{}
```
变量连续用
```
    @str='123',@number
    content:@@number //123
```


2. less的变量内嵌
```
@content:'str'
.box{
    &:after{
        content:@content 
    }
}
```

3. less的判断句(可以判断某些样式是否展示出来)
```
@toggle: true
.box when(@toggle = true) {
    color: red;
}
```

4. less的继承(可以通过extend来继承其他的class样式。)
```
.content1 {
    color:red
}
.content2 {
    &:extend(.content1)
    color:blue //这里起效果的是red,blue会被覆盖
}
```

```
.content2 {
    &.extend(.content1)
    color:blue //这里blue会覆盖上面的。
}
.content1 {
    color:red
}
```



