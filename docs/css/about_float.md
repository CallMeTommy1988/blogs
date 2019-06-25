# 关于清除浮动

## 什么是浮动?

首先什么是浮动，得说道 css 中的定位机制 **普通流，浮动，绝对定位** 。


##### 1. 普通流

只要你不是 float、absolutely positioned和root element 

##### 2. 浮动

> 浮动：浮动的框可以左右移动，直至它的外边缘遇到包含框或者另一个浮动框的边缘。浮动框不属于文档中的普通流，当一个元素浮动之后，不会影响到块级框的布局而只会影响内联框（通常是文本）的排列，文档中的普通流就会表现得和浮动框不存在一样，当浮动框高度超出包含框的时候，也就会出现包含框不会自动伸高来闭合浮动元素（“高度塌陷”现象）。顾名思义，就是漂浮于普通流之上，像浮云一样，但是只能左右浮动
##### 3. 绝对定位


正因为浮动元素的特性，他会浮在普通流元素之上。所以会遮住其他元素。

一般的解决办法就是 **清除浮动** 和  **闭合浮动** 

以前只听说**清除浮动**,其实不准确。

清除浮动严格意义上说并不能达到我们想要的效果，因为定义中的**清除浮动**,只是单纯的**清除浮动**，并不能解决

> 高度塌陷的问题

也就是有时候你其实已经**清除浮动**，但是`div`的高度依然为0的情况.

其实我们想要的是闭合浮动。浮动元素只影响到他本身。

## 清理浮动的方法

### 1. 增加元素

比如

```
<div style="clear:both;"></div>
<br clear="all">
```
放在浮动元素后，就可以**闭合浮动**

或者使用伪元素

```
.clearfix:after {content:"."; display:block; height:0; visibility:hidden; clear:both; }
.clearfix { *zoom:1; }
```

### 2. 通过父元素设置css

```
overflow:hidden
overflow:auto
display:table
float: xxxx 
```

也可以达到同样的效果.

## 原理

**直接增加元素**的方式是不太好的，因为会增加空标签，不管从语义上还是维护上都**不推荐**

###  Block formatting contexts

其实以前就有小姐姐问我 为什么 `overflow:hidden` 能清除浮动， 我当时告诉她我不知道。 后来也没放在心上，今天才知道这个事情的原理。

[Block formatting contexts][1](块级格式化上下文)
[1]: https://www.w3.org/TR/CSS21/visuren.html#block-formatting%20%20Block%20formatting%20contexts

#### 1. 块级格式化上下文会阻止外边距叠加

作为一个伪前端，居然不太知道这个东西。

> In CSS, the adjoining margins of two or more boxes (which might or might not be siblings) can combine to form a single margin. Margins that combine this way are said to collapse, and the resulting combined margin is called a collapsed margin.

> 在CSS中，两个或多个毗邻的普通流中的盒子（可能是父子元素，也可能是兄弟元素）在垂直方向上的外边距会发生叠加，这种形成的外边距称之为外边距叠加。

```
<div style="height:500px;width:100%;background-color:#2196f3;">
    <div style="margin: 20px; height: 100px; width:100px; background-color: red;">
    </div>
</div>
```

其实没有里面的
```
margin: 20px
```
的时候是正常的，但是一旦加入这个。 我发现父级的`div`也有了 `margin` 且子级的 `margin` 消失了和父级重叠了。
这就是所谓的**外边距叠加**

出现的条件

- 普通流
- 父子元素，也可能是兄弟元素
- 垂直方向
- 没有被`padding`、`border`、`clear`和`line box`分隔开

也就是说你使用这几个东西隔开就不会出现，以及 [Block formatting contexts][1](块级格式化上下文)

#### 2. 块级格式化上下文不会重叠浮动元素

只要你是 [Block formatting contexts][1](块级格式化上下文), 其他浮动的元素不会和你重叠。

#### 3. 块级格式化上下文通常可以包含浮动

也就是说，其他地方你需要 
```
clear:both
```

但是再  [Block formatting contexts][1](块级格式化上下文) 不需要，他会自动闭合浮动，自动撑满高度。

###  Block formatting contexts 的条件

- float 除了none以外的值 
- overflow 除了visible 以外的值（hidden，auto，scroll ） 
- display (table-cell，table-caption，inline-block) 
- position（absolute，fixed） 
- fieldset元素

## 值得一提

 [Block formatting contexts][1] 的条件中包含了 
 
 > float 除了none以外的值 
 
 也就是说父级别是浮动，后续就不用加其他 `clear:both`
 
 以及
 
 > display (table-cell，table-caption，inline-block) 
 
 前文中包含了
 
 > display:table
 
 但是他并不是 [Block formatting contexts][1]
 
 > display:table 本身并不会创建BFC，但是它会产生匿名框(anonymous boxes)，而匿名框中的display:table-cell可以创建新的BFC，换句话说，触发块级格式化上下文的是匿名框，而不是display:table。所以通过display:table和display:table-cell创建的BFC效果是不一样的。
 
## 最终的解决方案

其实说了这么多 [Block formatting contexts][1]，但是他并不是一个很好的解决方案。

因为往往他都需要改变特性。

- `overflow:hidden` 内容增多时候容易造成不会自动换行导致内容被隐藏掉，无法显示需要溢出的元素；04年POPO就发现overflow:hidden会导致中键失效
- `overflow:auto` 多个嵌套后，firefox某些情况会造成内容全选；IE中 mouseover 造成宽度改变时会出现最外层模块有滚动条等，firefox早期版本会无故产生focus等
- `父级别float` 影响布局
- `display:table` 盒模型属性改变

等等等等

```
/* For modern browsers */
.cf:before,.cf:after {
content:"";
display:table;
}
.cf:after { clear:both; }/* For IE 6/7 (trigger hasLayout) */
.cf { zoom:1; }
```

这个方式可以解决边界重叠，也可以闭合浮动，兼容了IE。

关于 

```
.cf { zoom:1; }
```

他可以触发 **IE** `hasLayout` 可以理解为 IE8以下的 [Block formatting contexts][1]。 只是现在这年头已经不用兼容 **IE8** 了，连我司的企业网站都至少 **IE10** 所以就不用去学去看了。

以上


## 链接

1. http://www.iyunlu.com/view/css-xhtml/55.html
2. https://tech.youzan.com/css-margin-collapse/
3. https://www.w3.org/TR/CSS2/visudet.html#root-height

以及谷歌翻译
 

