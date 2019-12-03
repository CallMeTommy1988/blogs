# css选择器

### 1. 为什么要再看选择器

其实从来没有系统的学习过css，都是知道基本的用法，边用边查。

其实还是应该知道一下，虽然其实大部分都知道 哈哈哈哈

### 2. 基础选择器

#### 2.1 * 通用选择器

代表所有

```
* { }
.demo * { }  
```

#### 2.2 E 元素选择器

```
li {  }
.demo li {  }
```

#### 2.3 .className 类选择器

```
.demo {  }
```

这里要补充一个用法，以前我并不知道虽然没有什么大碍。

```
<div id="bbbb" class="aaaa ccccc"></div>
```

用类选择器怎么选定呢？

以前我的做法是

```
父级别 .aaaa { }
```

其实还有一种用法是同级别的

```
#bbbb.aaaa { }
.ccccc.aaaa { }
```

都能选中

#### 2.4 id选择器

```
#id { }
```

#### 2.5 后代选择器（Ｅ Ｆ）

E F，前面E为祖先元素，Ｆ为后代元素，所表达的意思就是选择了Ｅ元素的所有后代Ｆ元素


```
#id .a { }
.a p { }
p .a { }
```

选中后代所有元素

#### 2.6 子元素选择器(E>F)

相较于 **2.5** 

所有的后代元素，用 `>` 分割以后只包含子元素 


```
#id > .a { }
.a > p { }
p > .a { }
```

#### 2.7 相邻兄弟元素选择器(E + F)

类似于 `next`

也只是 `next` 选择下一个元素. 如果下一个元素不是就不会生效

```
#id + div { }
#id + .aaaa { }
```

如果下一个不是指定的选择器不会继续遍历下一个。

#### 2.8 通用兄弟选择器（Ｅ 〜 Ｆ）

相较于 **2.8** 选择下一个指定元素，这个就不仅仅是选择下一个，后续所有的元素(选择器)。

```
    <div></div>
    <div id="bbbb" class="aaaa ccccc"></div>    
    <p></p>
    <div class="cccc">

    </div>
    <div>
        
    </div>
```
`+` 只能选择 `cccc` 这个 `div`

```
#bbbb + div {  }
```

`~` 就会选择到后续所有 `div`

```
#bbbb ~ div { }
```

#### 2.9 群组选择器

之前的所有选择器，都可以一起使用

```
.aaaa,#bbbb ~ div { }
```

### 3. 属性选择器

#### 3.1 	E[attr] 只匹配属性

```
a[href] { }
img[src] { }
```

只要你有这个属性，不管你有没有值。 还可以连续匹配

```
a[href][id] {  }
```

#### 3.2  Ｅ[attr="value"]

跟 **3.1** 相比增加了 `value` 的匹配，但是必须要完全匹配


```
input[type="text"][value="aaa"] { }
```

同样也可以多个同时选择.

#### 3.3  Ｅ[attr~="value"]

**3.2** 算是全字匹配。 这个就可以区分字来匹配

举个例子

```
<a href="http://www.baidu.com">http://www.baidu.com</a>
<a href="http://www.goodix.com">http://www.goodix.com</a>
<a href="http://google.com">http://google.com</a>
```

如果直接这样写是无法匹配的

```
a[href~="www"] {
    color:blue;
    background: red;
}
```

除非HTML是这样的

```
<a href="http://www.baidu.com www">http://www.baidu.com</a>
<a href="http://www.goodix.com">http://www.goodix.com</a>
<a href="http://google.com">http://google.com</a>
```

就能够匹配到第一个

#### 3.4 E[attr^="value"] 

以开头来匹配

```
a[href^="http://www"] {
    color:blue;
    background: red;
}
```

就能匹配到前两个元素


#### 3.5 E[attr$="value"]

和 **3.4** 正好相反，以末尾来匹配.

#### 3.6 E[attr*="value"]

**3.3** 是需要有空格分隔才能匹配，这种方式就是所有都匹配了

```
a[href*="www"] {
    color:blue;
    background: red;
}
```

就直接匹配到前两个.

#### 3.7 E[attr|="value"]

```
a[href|="www"] {
    color:blue;
    background: red;
}
```

匹配以 `www` 或者 `www-` 开头的属性

### 4 伪类

这个就太熟悉了。

- link a标签 还没有访问过 
- visited 当a标签访问过以后
- hover 鼠标移上去
- active 就是鼠标点击到放开的基本状态
- disable 注销的状态	
- enabled 启用状态	
- checked 选中状态
- focus 选中的基本状态

这几个都属于比较常用的。

```
a:visited {
    background: black;
    color: blue;
}

a:hover {
    background: blue;
    color: black;
}

a:active {
    background: blue;
    color: white;
}

input[type="text"]:disabled {
    background: blue;
}

input[type="text"]:enabled {
    background: red;
}

input[type="radio"]:checked {
    display: none;
}


<a href="#1">没有访问过的标签</a>
<a href="#">点击访问过的标签</a>
<a href="#2">鼠标移动上去</a>
<a href="#3">激活状态</a>

<p>
	<input type="text">
</p>

<p>
	<input type="text" disabled="disabled">
</p>

<p>
	<input type="radio">
</p>
```

这就是伪类的基本用法。 其实大同小异，只需要区分出场景即可。

唯一就是有一个优先级问题

> 
若没有a{}
若无a:link，a:link将采用默认的字体大小和颜色
若无a:hover，a:hover将继承a:link的所有属性。若有a:link，first-child:hover继承自己没有的属性。
若没有a:active，a:active将先继承a:hover中的所有属性，然后从a:link继承没有的属性。
若没有a:visited,  a:visited将采用默认的字体大小和颜色。
总结：优先级L->V->H->A。可以这样记LoVe HAte（爱恨）

这个是网上抄的，大概就是这样的集成关系.

### 5. nth选择器

#### 5.1 first-child

```
p:first-child { } 
```

其实蛮简单的，就是选中第一个P。

唯一值得注意的点，他是按照容器来算的。

```
<p></p>
<div>
	<p></p>
</div>
```

两个 `P` 都会选中。

也就容器中的第一 `p` 都会选中。

#### 5.2 last-child

和 **5.1 first-child** 一样。 只不过选择最后一个.

#### 5.3 nth-child

选择具体某一个，不过有集中形态

```
p:nth-child(1) {  }
```

选择第一个。 参数不能为负数。

还有一种写法 `n`

```
p:nth-child(n);
p:nth-child(2n);
```

可以理解为

```
n = 1
n = 2
....
```

然后就可以开始

里面的表达式可以自己计算

```
2n + 1
2*1 + 1 = 3
2*2 + 1 = 5
.....
```

这就是这个表达式的作用


#### 5.4 :nth-last-child

和 **5.3 nth-child** 差不多，只是反向选择罢了

从头到尾和从尾到头。

#### 5.5 :empty

选择空元素，也就是没有内容的元素。

PS: 空格也算是没有内容

#### 5.6 :not

就是增加了一个否的选择器

```
.demo p:not(attr="value")
```

只要匹配之后就无效。 

### 6. 伪元素

#### 6.1 ::first-line

第一行元素的样式

```
p::first-line { }
```

#### 6.2 ::first-letter

首字母样式

#### 6.3 after before

这两个主要用来给元素的前面或后面插入内容，这两个常用"content"配合使用，见过最多的就是清除浮动。




