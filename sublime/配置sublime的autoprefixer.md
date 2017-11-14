###配置autoPrefixer
1.sublime package install autoprefixer
2.preferences->key Bindings ->User  配置命令
```javascript   
{
    "keys":["ctrl+alt+p"],"command":"autoprefixer"
}
```

3.适用的浏览器规格
preference->package settings -> autoprefixer -> user
```javascript
{
    "browsers": ["last 5 version", "iOS 7"]
}

```

| 写法 | 解释| 
|------|-----|
|last 2 versions |每一个主要浏览器的最后2个版本|
|last 2 Chrome versions  |谷歌浏览器的最后两个版本|
|> 5%    |市场占有量大于5%|
|> 5% in US  |美国市场占有量大于5%|
|ie 6-8  |e浏览器6-8|
|Firefox > 20    |火狐版本>20|
|Firefox >= 20   |火狐版本>=20|
|Firefox < 20    |火狐<20|
|Firefox <= 20   |火狐<=20|
|iOS 7   |指定IOS 7浏览器|