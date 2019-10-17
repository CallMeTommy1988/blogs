# text-overflow

---

### 1. 基本

text-overflow 都知道

```
text-overflow: clip|ellipsis|string;
```

- clip 修剪
- ellipsis 省略号
- string 使用给定的字符串来代表被修剪的文本

### 2. 浏览器支持

全部支持，连IE6都支持.

### 3. 应用

```
div {
	width: 150px;
	overflow: hidden;
	border: 1px solid #ccc;
}
```

一切都需要在 `overflow:hidden` 才能起作用

#### 3.1 单行基础

```
white-space:nowrap;
text-overflow: ellipsis;
```

设置一行显示 `white-space:nowrap`

这个时候再设置 **省略号** 显示，就能实现单行超出 ...

问题是每个浏览器单行显示都不一样。比如 **firefox** 的省略号就不一样

#### 3.2 单行 js

`text_overflow.js`

可以实现单行省略号，并且保持浏览器下显示一致。

```
$(".demo3").attr("displayLength","20");
$(".demo3 ").text_overflow();

$(".demo_1").attr("displayLength","20");
$(".demo_1").text_overflow();
```

问题就是只能限制字数，在多语言下很恶心。而且需要引入 `jquery`

#### 3.3 多行

原本不支持多行，但是在 **WebKit** 下可以利用浏览器特性进行支持。

```
text-overflow: ellipsis;
display: -webkit-box;
-webkit-line-clamp: 3;
-webkit-box-orient: vertical;
```
使用 `-webkit-line-clamp` 来规定显示多少行。

但是问题也很明显。

非 **WebKit** 内核不支持，比如**IE**.

#### 3.4 多行  js

`jquery.dotdotdot.js`

非常完美。完全根据你设置的高度来进行，你只需要设置

```
$('.demo_2').dotdotdot();
```

唯一的问题是，你需要在页面大小切换的时候，再次调用这个函数。

#### 3.5 多行 css + js

在确定 `div` 高度的情况下，使用绝对定位来解决这个问题

```
overflow: hidden;
height: 50px;
position: relative;

<div class="demo_3">
<span style="position: absolute; top:27px; left:129px; border: none; background: white;">....</span>
 达市啊安达市大所大打算爱的爱的爱的爱仕达爱仕达爱是大声道爱仕达爱是大
</div>
```

这个是比 **3.4** 其实还要更复杂，因为当页面变化，需要更多代码去解决屏幕问题。

### 4. 终极解决

其实就是 **3.1,3.3,3.4** 结合

默认写 **3.3** 当非 **WebKit** 内核的时候，通过`javascript` 来解决调用。

当然这也不是最好的方法 

因为 **3.4**  需要引入 `jquery`， 如果你没有引用`jquery`，建议就是去翻译一下 `jquery.dotdotdot.js` 改造为非 `jquery` 代码

以上


