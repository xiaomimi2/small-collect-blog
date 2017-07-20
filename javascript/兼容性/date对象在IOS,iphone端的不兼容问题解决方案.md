###兼容safari的Date对象的写法

常见的创建的日期方式
```
new Date("month dd, yyyy hh:mm:ss")
new Date("moth dd,yyyy")
new Date(yyyy,mth,dd,hh,mm,ss) //注意这个是number类型传入，不是字符串
new Date(yyyy,mth,dd)
new Date(ms)
```

iphone只支持的格式是
`new Date(YYYY,MM,DD,HH,mm,ss)`