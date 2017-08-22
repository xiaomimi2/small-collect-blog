
##135集成uEditor作为插件使用
1.135editor.js,放在ueditor.config.js的同级目录下的plugin目录下。 这里涉及editor.options.UEDITOR_HOME_URL这个配置项。这里涉及了ueditor的二次开发。
```javascript  
UE.registerUI('135editor', function(editor, uiName) {
    var dialog = new UE.ui.Dialog({
        iframeUrl: editor.options.UEDITOR_HOME_URL + 'dialogs/135editor/135EditorDialogPage.html',
        editor: editor,
        name: uiName,
        title: "135编辑器"
    });
    dialog.fullscreen = true;
    dialog.draggable = false;
    var btn = new UE.ui.Button({
        name: 'btn-dialog-' + uiName,
        className: 'edui-for-135editor',
        title: '135编辑器',
        onclick: function() {
            dialog.render();
            dialog.open();
        }
    });
    return btn;
}, undefined); //undefined默认放在最后，填上具体数字就放到具体的工具栏的某一个部分
```


2.135EditorPage.html这个开发文档上要求的文件下来。具体放置在的位置和上述135editor.js的代码相关。`editor.options.UEDITOR_HOME_URL + 'dialogs/135editor/135EditorDialogPage.html',`

3..ueditor.config.js本身需要进行更改。主要修改的关于过滤方面的内容。都是防止xss攻击的。过滤内容注释即可。
```javascript
,xssFilterRules: true
        //input xss过滤
        ,inputXssFilter: true
        //output xss过滤
        ,outputXssFilter: true
        // xss过滤白名单 名单来源: https://raw.githubusercontent.com/leizongmin/js-xss/master/lib/default.js
        ,whitList: {
            a:      ['target', 'href', 'title', 'class', 'style'],
            abbr:   ['title', 'class', 'style'],
            address: ['class', 'style'],
            area:   ['shape', 'coords', 'href', 'alt'],
            article: [],
            aside:  [],
            audio:  ['autoplay', 'controls', 'loop', 'preload', 'src', 'class', 'style'],
            b:      ['class', 'style'],
            bdi:    ['dir'],
            bdo:    ['dir'],
            big:    [],
            blockquote: ['cite', 'class', 'style'],
            br:     [],
            caption: ['class', 'style'],
            center: [],
            cite:   [],
            code:   ['class', 'style'],
            col:    ['align', 'valign', 'span', 'width', 'class', 'style'],
            colgroup: ['align', 'valign', 'span', 'width', 'class', 'style'],
            dd:     ['class', 'style'],
            del:    ['datetime'],
            details: ['open'],
            div:    ['class', 'style'],
            dl:     ['class', 'style'],
            dt:     ['class', 'style'],
            em:     ['class', 'style'],
            font:   ['color', 'size', 'face'],
            footer: [],
            h1:     ['class', 'style'],
            h2:     ['class', 'style'],
            h3:     ['class', 'style'],
            h4:     ['class', 'style'],
            h5:     ['class', 'style'],
            h6:     ['class', 'style'],
            header: [],
            hr:     [],
            i:      ['class', 'style'],
            img:    ['src', 'alt', 'title', 'width', 'height', 'id', '_src', 'loadingclass', 'class', 'data-latex'],
            ins:    ['datetime'],
            li:     ['class', 'style'],
            mark:   [],
            nav:    [],
            ol:     ['class', 'style'],
            p:      ['class', 'style'],
            pre:    ['class', 'style'],
            s:      [],
            section:[],
            small:  [],
            span:   ['class', 'style'],
            sub:    ['class', 'style'],
            sup:    ['class', 'style'],
            strong: ['class', 'style'],
            table:  ['width', 'border', 'align', 'valign', 'class', 'style'],
            tbody:  ['align', 'valign', 'class', 'style'],
            td:     ['width', 'rowspan', 'colspan', 'align', 'valign', 'class', 'style'],
            tfoot:  ['align', 'valign', 'class', 'style'],
            th:     ['width', 'rowspan', 'colspan', 'align', 'valign', 'class', 'style'],
            thead:  ['align', 'valign', 'class', 'style'],
            tr:     ['rowspan', 'align', 'valign', 'class', 'style'],
            tt:     [],
            u:      [],
            ul:     ['class', 'style'],
            video:  ['autoplay', 'controls', 'loop', 'preload', 'src', 'height', 'width', 'class', 'style']
```