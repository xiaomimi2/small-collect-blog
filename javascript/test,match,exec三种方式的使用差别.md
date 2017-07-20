##test,match,exec三种方式的使用差别

###test
返回值是布尔值,regExp在前面
```javascript
var str = '1a1b1c';
/1./.test(str)
```
结果是true

###exec
```javascript
var str = '1a1b1c';
var req = new RegExp("1.",'g');
console.log(req.exec(str));
console.log(req.exec(str));
```
结果是`["1a", index: 0, input: "1a1b1c"]
VM2481:4 ["1b", index: 2, input: "1a1b1c"]`
这里有个奇怪的就是直接用/1./g来测试两次结果都是1a


###match

```javascript
var str = '1a1b1c';
var req = new RegExp("1.",'g');
str.match(req)
```

结果是`["1a", "1b", "1c"]`
