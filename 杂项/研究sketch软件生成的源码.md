###sketch生成的文件的源码问题记录
![设计分辨率选项](https://github.com/xiaomimi2/small-collect-blog/blob/master/image/ratio.png)
1.这里的this.config.scale就是选项标签自带的属性
```html
<li><label><input type="radio" name="resolution" data-name="高清视网膜 @3x" data-unit="pt" data-scale="3"><span>高清视网膜 @3x</span></label>
</li>
```

2.根据input\[name=resolution\]找到一下代码。对应的就是上面可切换的像素类型
```javascript
$('#unit')
    .on('change', 'input[name=resolution]', function(){
        var $checked = $('input[name=resolution]:checked');
        self.configs.unit = $checked.attr('data-unit');
        self.configs.scale = Number( $checked.attr('data-scale') );
        self.targetID = undefined;
        self.layers();
        self.inspector();
        $('#unit')
            .blur()
            .find('p')
            .text(
                $checked.attr('data-name')
                );
        self.slices();
    })
```


3.知道layer()函数
```javascript
layers: function() {
    var self = this,
        layersHTML = [];
    $.each(this.current.layers, function(index, layer) {
        var x = self.zoomSize( layer.rect.x ),
            y = self.zoomSize( layer.rect.y ),
            width = self.zoomSize( layer.rect.width ),
            height = self.zoomSize( layer.rect.height ),
            classNames = ['layer'];
            if(index == 1) console.log('pre', layer.rect.x,'now',x,'w',layer.rect.width,'w',width)
        classNames.push('layer-' + layer.objectID);
        if(self.selectedIndex == index) classNames.push('selected');
        layersHTML.push([
            '<div id="layer-' + index + '" class="' + classNames.join(' ') + '" data-index="' + index + '" percentage-width="' + self.percentageSize(layer.rect.width, self.current.width) + '" percentage-height="' + self.percentageSize(layer.rect.height, self.current.height) + '" data-width="' + self.unitSize(layer.rect.width) + '" data-height="' + self.unitSize(layer.rect.height) + '" style="left: ' + x + 'px; top: ' + y + 'px; width: ' + width + 'px; height: ' + height + 'px;">',
                '<i class="tl"></i><i class="tr"></i><i class="bl"></i><i class="br"></i>',
                '<b class="et h"></b><b class="er v"></b><b class="eb h"></b><b class="el v"></b>',
            '</div>'
        ].join(''));
    });
    $('#layers').html(layersHTML.join(''));
},
```
可以看到尺寸换算主要是使用了zoomSize()这个方法

4.zoomsize函数
```javascript
zoomSize: function(size) {
            return (size * this.configs.zoom);
        },

//关于this.configs.zoom的代码片段
var proportion = $(document).height() / this.current.height;
if (proportion >= .8) {
    this.configs.zoom = 1;
} else if (proportion >= .7) {
    this.configs.zoom = .75;
} else {
    this.configs.zoom = .5;
}
```

5.知道标注的距离的相关部分,也是和zoomscale有关系。
```javascript
distance: function(){//
    if( ( this.selectedIndex && this.targetIndex && this.selectedIndex != this.targetIndex ) || ( this.selectedIndex && this.tempTargetRect ) ){
        var selectedRect = this.getRect(this.selectedIndex),
            targetRect = this.tempTargetRect || this.getRect(this.targetIndex),
            topData, rightData, bottomData, leftData,
            x = this.zoomSize(selectedRect.x + selectedRect.width / 2),
            y = this.zoomSize(selectedRect.y + selectedRect.height / 2);
            console.log(selectedRect.x + selectedRect.width / 2, 'distance', x)
        if(!this.isIntersect(selectedRect, targetRect)){
            if(selectedRect.y > targetRect.maxY){ //topData
                topData = {
                    x: x,
                    y: this.zoomSize(targetRect.maxY),
                    h: this.zoomSize(selectedRect.y - targetRect.maxY),
                    distance: this.unitSize(selectedRect.y - targetRect.maxY),
                    percentageDistance: this.percentageSize((selectedRect.y - targetRect.maxY), this.current.height)
                };
            }
            if(selectedRect.maxX < targetRect.x){ //right
                rightData = {
                    x: this.zoomSize(selectedRect.maxX),
                    y: y,
                    w: this.zoomSize(targetRect.x - selectedRect.maxX),
                    distance: this.unitSize(targetRect.x - selectedRect.maxX),//比如这里就是标注的尺寸的distance
                    percentageDistance: this.percentageSize((targetRect.x - selectedRect.maxX), this.current.width)
                }
            }
            if(selectedRect.maxY < targetRect.y){ //bottom
                bottomData = {
                    x: x,
                    y: this.zoomSize(selectedRect.maxY),
                    h: this.zoomSize(targetRect.y - selectedRect.maxY),
                    distance: this.unitSize(targetRect.y - selectedRect.maxY),
                    percentageDistance: this.percentageSize((targetRect.y - selectedRect.maxY), this.current.height)
                }
            }
            if(selectedRect.x > targetRect.maxX){ //left
                leftData = {
                    x: this.zoomSize(targetRect.maxX),
                    y: y,
                    w: this.zoomSize(selectedRect.x - targetRect.maxX),
                    distance: this.unitSize(selectedRect.x - targetRect.maxX),
                    percentageDistance: this.percentageSize((selectedRect.x - targetRect.maxX), this.current.width)
                }
            }
        }
        else{
            var distance = this.getDistance(selectedRect, targetRect);
            if (distance.top != 0) { //top
                topData = {
                    x: x,
                    y: (distance.top > 0)? this.zoomSize(targetRect.y): this.zoomSize(selectedRect.y),
                    h: this.zoomSize(this.positive(distance.top)),
                    distance: this.unitSize(this.positive(distance.top)),
                    percentageDistance: this.percentageSize(this.positive(distance.top), this.current.height)
                };
            }
            if (distance.right != 0) { //right
                rightData = {
                    x: (distance.right > 0)? this.zoomSize(selectedRect.maxX): this.zoomSize(targetRect.maxX),
                    y: y,
                    w: this.zoomSize(this.positive(distance.right)),
                    distance: this.unitSize(this.positive(distance.right)),
                    percentageDistance: this.percentageSize(this.positive(distance.right), this.current.width)
                };
            }
            if (distance.bottom != 0) { //bottom
                bottomData = {
                    x: x,
                    y: (distance.bottom > 0)? this.zoomSize(selectedRect.maxY): this.zoomSize(targetRect.maxY),
                    h: this.zoomSize(this.positive(distance.bottom)),
                    distance: this.unitSize(this.positive(distance.bottom)),
                    percentageDistance: this.percentageSize(this.positive(distance.bottom), this.current.height)
                };
            }
            if (distance.left != 0) { //left
                leftData = {
                    x: (distance.left > 0)? this.zoomSize(targetRect.x): this.zoomSize(selectedRect.x),
                    y: y,
                    w: this.zoomSize(this.positive(distance.left)),
                    distance: this.unitSize(this.positive(distance.left)),
                    percentageDistance: this.percentageSize(this.positive(distance.left), this.current.width)
                };
            }
        }
        if (topData) {
            $('#td')
                .css({
                    left: topData.x,
                    top: topData.y,
                    height: topData.h,
                })
                .show();
            $('#td div')
                .attr('data-height', topData.distance)
                .attr('percentage-height', topData.percentageDistance);
        }
        if (rightData) {
             $('#rd')
                .css({
                    left: rightData.x,
                    top: rightData.y,
                    width: rightData.w
                })
                .show();
            $('#rd div')
                .attr('data-width', rightData.distance )
                .attr('percentage-width', rightData.percentageDistance);
        }
        if (bottomData) {
            $('#bd')
                .css({
                    left: bottomData.x,
                    top: bottomData.y,
                    height: bottomData.h,
                })
                .show();
            $('#bd div')
                .attr('data-height', bottomData.distance )
                .attr('percentage-height', bottomData.percentageDistance);
        }
        if (leftData) {
             $('#ld')
                .css({
                    left: leftData.x,
                    top: leftData.y,
                    width: leftData.w
                })
                .show();
            $('#ld div')
                .attr('data-width', leftData.distance )
                .attr('percentage-width', leftData.percentageDistance);
        }
        $('.selected').addClass('hidden');
    }
},
```


6.标注尺寸的函数
```javascript
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



7.最后修改的地方可以很简单，直接是
```javascript   
$.each(unitsData, function(index, data){
    if(data.name) unitList.push('<li class="sub-title">' + _(data.name) + '</li>');
    $.each(data.units, function(index, unit){
        var checked = '';
        // if(unit.scale == self.configs.scale){
            if( unit.unit == self.configs.unit && unit.scale == self.configs.scale ){
                checked = ' checked="checked"';
                hasCurrent = _(unit.name);
            }
            unitList.push('<li><label><input type="radio" name="resolution" data-name="' + _(unit.name) + '" data-unit="' + unit.unit + '" data-scale="' + unit.scale + '"' + checked + '><span>' + _(unit.name) + '</span></label></li>');
        // }
    });
});

```

```javascript
unitsData = [
    {
        units: [
            {
                name: _('Standard'),
                unit: 'px',
                scale: 1
            }
        ]
    },
    {
        name: _('iOS Devices'),
        units: [
            {
                name: _('Points') + ' @1x',
                unit: 'pt',
                scale: 1
            },
            {
                name: _('Retina') + ' @2x',
                unit: 'pt',
                scale: 2
            },
            {
                name: _('Retina HD') + ' @3x',
                unit: 'pt',
                scale: 3
            }
        ]
    },
    {
        name: _('Android Devices'),
        units: [
            {
                name: 'LDPI @0.75x',
                unit: 'dp/sp',
                scale: .75
            },
            {
                name: 'MDPI @1x',
                unit: 'dp/sp',
                scale: 1
            },
            {
                name: 'HDPI @1.5x',
                unit: 'dp/sp',
                scale: 1.5
            },
            {
                name: 'XHDPI @2x',
                unit: 'dp/sp',
                scale: 2
            },
            {
                name: 'XXHDPI @3x',
                unit: 'dp/sp',
                scale: 3
            },
            {
                name: 'XXXHDPI @4x',
                unit: 'dp/sp',
                scale: 4
            }
        ]
    },
    {
        name: _('Web View'),
        units: [
            {
                name: 'CSS Rem 12px',
                unit: 'rem',
                scale: 12
            },
            {
                name: 'CSS Rem 14px',
                unit: 'rem',
                scale: 14
            },
            {
                name: 'CSS Rem 16px',
                unit: 'rem',
                scale: 16
            },
            {
                name: 'CSS Rem 100px',
                unit: 'rem',
                scale: 100
            }
        ]
    }
],
```


所以最后的方法只要在unitsData中加入这个一条数据即可，请看上述多了一个100px的比例值。
