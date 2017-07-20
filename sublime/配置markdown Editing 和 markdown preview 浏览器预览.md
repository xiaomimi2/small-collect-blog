## 配置markdown Editing 和 markdown preview 浏览器预览

package install markdown Editing和 markdown preview
然后打开preference下的key binding 配置如下。之后就可以实现直接ctrl+m键预览md文档结果
```javascript
[
      {
        "keys": ["ctrl+m"], "command": "markdown_preview",
        "args": {"target": "browser"}
    }
]
```