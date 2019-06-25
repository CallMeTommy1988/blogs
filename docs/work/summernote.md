# summernote 的一些坑

## 分析


### 1. toolbar

> **Insert**
picture: open image dialog 
link: open link dialog
video: open video dialog
table: insert a table
hr: insert a horizontal rule
**Font Style**
fontname: set font family
fontsize: set font size
color: set foreground and background color
forecolor: set foreground color
backcolor: set background color
bold: toggle font weight
italic: toggle italic
underline: toggle underline
strikethrough: toggle strikethrough
superscript: toggle superscript
subscript: toggle subscript
clear: clear font style
**Paragraph style**
style: format selected block
ol: toggle ordered list
ul: toggle unordered list
paragraph: dropdown for paragraph align
height: set line height
**Misc**
fullscreen: toggle fullscreen editing mode
codeview: toggle wysiwyg and html editing mode
undo: undo
redo: redo
help: open help dialog

其实基本囊括了大部分。

他还可以分组

```
//文字处理
[
    'txt', [
        'fontsize', //字体大小
        'italic', //斜体
        'underline', //下划线
        'strikethrough' //删格线
    ]
],
```

你还可以自定义。

```
var HelloButton = function (context) {
    var ui = $.summernote.ui;

    // create button
    var button = ui.button({
        contents: '<i class="fa fa-child"/> 自定义的按钮',
        tooltip: 'hello',
        click: function () {
            // invoke insertText method with 'hello' on editor module.
            $('#summernote').summernote('insertText', 'hello');
        }
    });

    return button.render();   // return button as jquery object
}

$('.summernote').summernote({
  toolbar: [
    ['mybutton', ['hello']]
  ],

  buttons: {
    hello: HelloButton
  }
});
```

在文档中 `helloButton` 是有参数的，能传入当前的 `editor` 对象，但是我调试了源码并不会。 只能受限制。 也就是你在封装的过程当中最好保留一个变量。

### 2. 上传图片

默认支持 `base64` 。

但是这边希望上传图片能够上传到服务器，以便更好的控制和压缩图片大小。

```
onImageUpload: function (files, editor, welEditable) {
    data = new FormData();
    data.append("image", files[0]);
    url = "/upload";
    $.ajax({
        data: data,
        type: "POST",
        url: url,
        cache: false,
        contentType: false,
        processData: false,
        success: function (url) {
            $("#summernote").summernote("insertImage", url);
        }
    });
}
```

### 3. 不允许粘贴带格式的html
```
onPaste: function (e) {
    var bufferText = ((e.originalEvent || e).clipboardData || window.clipboardData).getData('Text');
    e.preventDefault();
    document.execCommand('insertText', false, bufferText);
}
```

### 4. 自定义orderlist

这是一个及其特殊的需求。也就是需要重新 `orderlist` 按钮

需求是需要按照一段特殊的HTML，允许生成无限级的 `list`

```
<ul class="level1">
    <li class="lv1"><p></p></li>
    <ul class="level2">
    .....
</ul>
```

以此类推,有一个比较恶心的点是只有第一层有 `p`

首先看一下他自己怎么做的

```
Bullet.prototype.toggleList = function (listName, editable) {
    var _this = this;
    var rng = range.create(editable).wrapBodyInlineWithPara(); //获取选区
    var paras = rng.nodes(dom.isPara, { includeAncestor: true }); //判断需要多少操作的节点
    var bookmark = rng.paraBookmark(paras); 
    var clustereds = lists.clusterBy(paras, func.peq2('parentNode')); //筛选出节点
    // paragraph to list
    if (lists.find(paras, dom.isPurePara)) { //不是list，设置list
        var wrappedParas_1 = [];
        $$1.each(clustereds, function (idx, paras) { //遍历
            wrappedParas_1 = wrappedParas_1.concat(_this.wrapList(paras, listName)); //生成节点
        });
        paras = wrappedParas_1;
        // list to paragraph or change list style
    }
    else { //是list，取消list
        var diffLists = rng.nodes(dom.isList, {
            includeAncestor: true
        }).filter(function (listNode) {
            return !$$1.nodeName(listNode, listName);
        });
        if (diffLists.length) {
            $$1.each(diffLists, function (idx, listNode) {
                dom.replace(listNode, listName);
            });
        }
        else {
            paras = this.releaseList(clustereds, true);
        }
    }
    range.createFromParaBookmark(bookmark, paras).select();
};


Bullet.prototype.wrapList = function (paras, listName) {
    var head = lists.head(paras);
    var last = lists.last(paras);
    var prevList = dom.isList(head.previousSibling) && head.previousSibling;
    var nextList = dom.isList(last.nextSibling) && last.nextSibling;
    var listNode = prevList || dom.insertAfter(dom.create(listName || 'UL'), last); //创建list，并且插入在目标节点下方
    // P to LI
    paras = paras.map(function (para) { //批量将目标节点替换为LI
        return dom.isPurePara(para) ? dom.replace(para, 'LI') : para;
    });
    // append to list(<ul>, <ol>)
    dom.appendChildNodes(listNode, paras); //插入节点
    if (nextList) { //判断是否还有多的list
        dom.appendChildNodes(listNode, lists.from(nextList.childNodes));
        dom.remove(nextList);
    }
    return paras;
};
```

以上就是他的逻辑。

按照我的逻辑首先

1. 增加样式
2. 删除取消`list`的逻辑
3. 判断层级

```
var head = lists.head(paras);
if (head.nodeName.toUpperCase() === "P" && head.parentNode.nodeName.toUpperCase() === "LI") {//修正选区为LI
    head = head.parentNode;
}

var last = lists.last(paras);
if (last.nodeName.toUpperCase() === "P" && last.parentNode.nodeName.toUpperCase() === "LI") {//修正选区为LI
    last = last.parentNode;
}

var prevList = dom.isList(head.previousSibling) && head.previousSibling;
var nextList = dom.isList(last.nextSibling) && last.nextSibling;

//判断等级
var service = new CreateCustomOrderList(listName);
var curLv = service.getCurLv(head); //获取当前等级
var listNode = prevList || service.creatList(curLv, last); //通过当前等级去创建对应的 list

// P to LI
paras = paras.map(function (para) {
    if (dom.isPurePara(para)) {
        return service.createLi(curLv, para); //更具等级创建对应LI
    }
    else
        return para;
});
dom.appendChildNodes(listNode, paras);
if (nextList) {
    dom.appendChildNodes(listNode, lists.from(nextList.childNodes));
    dom.remove(nextList);
}

wrappedParas_1 = wrappedParas_1.concat(paras);
```

创建的代码

```
function CreateCustomOrderList(listName) {
    return {
        getCurLv: function (head) {
            var curLv = 1;
            if (head.parentNode.nodeName.toUpperCase() === "OL" ||
                head.parentNode.nodeName.toUpperCase() === "UL") {
                var lv = $(head.parentNode).attr("lv");
                if (lv) {
                    curLv = parseInt(lv) + 1;
                }
            }
            return curLv;
        },
        creatList: function (lv, last) {
            var list = dom.create(listName || 'UL');
            lv === 1 ? dom.insertAfter(list, last) : last.append(list);
            $(list).addClass("level" + lv);
            $(list).attr("lv", lv);
            return list;
        },
        createLi: function (curLv, para) {
            var LI;
            if (curLv == 1) {
                LI = dom.replace(para, 'LI');
                LI.innerHTML = "<p>" + LI.innerHTML + "</p>";
            }
            else if (curLv > 1 && para.nodeName.toUpperCase() == "P") {
                LI = dom.create("LI");
                var P = dom.create("P");
                P.innerHTML = para.innerHTML;
                LI.append(P);
            }
            else {
                LI = dom.create("LI");
                LI.innerHTML = para.innerHTML;
            }

            $(LI).addClass("lv" + curLv);
            return LI;
        }
    };
}
```

目前我是在外部注册，因为按钮是动态的。最好的是能够在 `Jquery` 插件中去注册，能够最大限度保持原有结构.

### 5. 关于大小

由于原有的项目中已经使用了 `jquery bootstrap` 所以只需要引入 `summernote.js & summernote.css` 即可。

加起来大概在 **140kb** 左右。 `gzip` 压缩后应该能控制在 **40kb** 左右 

### 6. 浏览器兼容性

经过测试兼容到 IE10. 非常好

### 7. 扩展性

理论上扩展性是无限的。 因为能够直接拿到选区。你想干什么都可以

```
var rng = range.create(editable).wrapBodyInlineWithPara(); 
```
在这个基础上做一系列其他操作

### 8. 关于所见即所得

由于 **summernote** 并不是在 `iframe` 的基础上做的编辑器。所以会有一个问题，无法单独引用 `css & js` 我搜索遍了文档和google 都无法找到解决方案。

最后只能用土办法

观察后发现 **summernote** 创建之后都会生成 `.note-editing-area` 来包裹编辑主窗体(不包toolbar). 将你需要应用的`css` 使用 `scss` 打开

```
.note-editing-area {
    ...你的css
}
```

然后编译，引用。 解决问题

## 9. 总结

- 插件还是不够灵活，很多内置函数并不能直接调用。导致了很难不在破坏完整性的情况下进行开发。如你自己愿意修改源码去稍微公开一些方法，那么就很好开发
- 源码功能不够强大，如果想要解决需要引入 `codemirror` 这其实又是一笔开销
- 偏笨重，因为这次的项目中已经包含了 `Jquery & bootstrap` 如果没有，整体的开销不会小。 如果你的项目中没有这两项不建议选这个编辑器
- 不能直接引用编辑器内的css

总体来说还是有些问题，但是很适合这次的项目，功能齐全且对于这个项目来说，增加很少，总体来说还算易于二次开发。所以最后选择了 `summtenote`

以上





