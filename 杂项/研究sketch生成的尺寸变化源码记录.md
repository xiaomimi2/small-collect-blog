###sketch生成的文件的源码问题记录
1.尺寸单位换算的函数
```
unitSize: function(length, isText){
    var length = Math.round( length / this.configs.scale * 100 ) / 100,
        units = this.configs.unit.split("/"),
        unit = units[0];
    if( units.length > 1 && isText){
        unit = units[1];
    }
    console.log('unitSize')
    return length + unit;
},
```
这里的this.config.scale就是选项标签自带的属性
![设计分辨率选项](https://github.com/xiaomimi2/small-collect-blog/blob/master/image/ratio.png)
```
<li><label><input type="radio" name="resolution" data-name="高清视网膜 @3x" data-unit="pt" data-scale="3"><span>高清视网膜 @3x</span></label>
</li>
```