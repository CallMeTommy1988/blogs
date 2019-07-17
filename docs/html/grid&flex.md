# 关于flex 和 Grid

## 1. 开头

今天看到有文章介绍 `Grid` ，其中讲了 `flex` 和 `Grid` 的区别，突然想起了好久没些 `flex`. 

先复习下 `flex`，再看 `Grid` ，然后对比差异。

## 2. flex

> 2009年，W3C 提出了一种新的方案----Flex 布局，可以简便、完整、响应式地实现各种页面布局。目前，它已经得到了所有浏览器的支持，这意味着，现在就能很安全地使用这项功能。

首先任何布局都能使用 `flex`

![aaa](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071004.png)

子布局的高和宽可以自己设定，高必须设定。不然不会显示出来。

关于宽总共有3种设置方式

```
.child_flex {
    width: 400px;
    width: 24%;
    flex: n;
}
```
高度的设置和普通css写法无异。


### 2.1 flex-direction

用于确定方向 `main axis` 或者 `cross axis`.
可以正序可以反序。

当你使用 `flex-direction: column` 的时候，需要设置父容器的高度。

这个很重要。 使用这个定义主轴后，另一条轴就是交叉轴。

正常情况下，也就是　`flex-direction : row`　的情况下

上左右是主轴，左上下是交叉轴

如果反过来　`flex-direction:row-reverse`

上右左是主轴，右上下是交叉轴.

`flex-direction:ｃｏｌｕｍｎ`同理

以左上下为主轴，那么交叉轴依然是上左右

`flex-direction:column-reverse` 

已左下上为主轴，已下左右为交叉轴。

这个顺序很重要。

### 2.2 flex-wrap

换行的方式，默认不换行，你设置可以按照上下来设置换行。

换行也根据你排序的方式改变。

```
flex-wrap: wrap;
```

你设置换行有一个前提。 你不能设置为

```
flex: 1;
```

不然永远不会换行.

### 2.3 flex-flow

属于 **2.1 & 2.2** 的简写，不用一个一个写.

### 2.4 justify-content

`main axis` 的对齐方式.

 `justify-content: flex-start | flex-end | center | space-between | space-around; `
 
1. `flex-start` 默认，左到右。
2. `flex-end` 右到左。
3. `center` 居中
4. `space-between` 两端对齐，项目之间的间隔都相等。
5. `space-around` 每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。

这里是主轴的对其方式，你的主轴发生变化，他也跟这变化

### 2.5 align-items

定义项目在交叉轴上如何对齐。

```
.box { align-items: flex-start | flex-end | center | baseline | stretch; } 
```
1. flex-start 以上为开始
2. flex-end 以下为开始
3. center 上下居中
4. baseline 项目的第一行文字的基线对齐。
5. stretch 填满

### 2.6 align-content

上下多轴的对其方式，其实和 `align-items` 类似，只是 `align-content` 应用于多轴.

```
.box { 
    align-content: flex-start | flex-end | center | space-between | space-around | stretch; 
    } 
```

1. space-between 两端对齐，中间间隔相等
2. space-around 每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍
3. stretch 填充

### 2.7 order

```
 .item { order: <integer>; } 
```

设置顺序

### 2.8 flex-grow

按照比例放大。

```
 .item { flex-grow: <number>; /* default 0 */ } 
```

放大的公式很简单。

比如 `div` 宽度　`100px`

你一个 `div`  `20px` 另一个　`30px`

那么剩余的宽度是`50px`

公式就是

```
(剩余宽度 / 你设置的 `flex-grow`总量) * 当前div的flex-grow
```



### 2.9 flex-shrink

按照比例缩放，默认为1.

设置为0的话，就是没有缩放，增加了会增加缩放。

默认为1的话就是等比缩放.

单独设置缩放比例

### 2.10 flex-basis

> 属性定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小。

自定义填充大小。 优先级会比较高

### 2.11 align-self

> align-self属性允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。

单独设置某一个 `item` 。

### 2.2 flex的总结

其实Flex的概念来说，不是特别好理解。

2个轴，主轴和交叉轴。

主轴的对齐方式，以及交叉轴的对齐方式（需要特别注意的是交叉轴可以设置单行和多行设置）

还有伸展的方式，以及缩放的方式。

你还可以单独设置某一个的交叉轴对其方式.

说简单其实也简单，说复杂你不好好理解也理解不透彻

## 3. Grid

`Grid` 简单看了一下，确实和 `flex` 很大的区别。

`Grid` 更适合二维布局以及一些不规则布局，虽然其实也能使用 `flex` 写出来不过会麻烦很多.

### 3.1 display 
```
display: grid
```

声明这个容器为 `grid`

### 3.2 grid-template-columns,grid-template-rows

```
.container {
  display: grid;
  grid-template-columns: 100px 100px 100px;
  grid-template-rows: 100px 100px 100px;
}
```

规定 `Grid` 下 `item` 的大小，`rows` 为竖轴, `columns` 为横轴。

#### 3.2.1 百分比

不仅仅可以使用 **100px** 这种精确的单位，也可以使用百分比.

#### 3.2.2 repeat

如果这个列很长的时候，会写很多 **100px** 或者 **百分比**. 使用 `repeat(100px)`

#### 3.2.3 fr

`fr` 可以理解为 `auto`.

```
//1fr 各占  1/3
grid-template-columns: 1fr 1fr 1fr;
grid-template-columns: auto auto auto;

//2fr 占 1/2 其他各站 1/4
grid-template-columns: 1fr 2fr 1fr;
grid-template-columns: auto auto * 2 auto;
```

而且 `fr` 可以与固定单位共用.

```
.container {
  display: grid;
  grid-template-columns: 150px 1fr 2fr;
}
```

在整体 `width - 150px`  后 `1fr` 占用 **1/3** , `2fr` 占用 **2/3**.

百分比也一样 **减去百分比**之后再算.

#### 3.2.4 其他运算。

`calc`，`minmax` 等等。 都可以运用到 `1fr`的运算中

### 3.3 grid-gap

行间距。

不用再去 `margin`，只需要设置 `grid` 即可.

```
grid-gap: <grid-row-gap> <grid-column-gap>;
grid-gap: 20px
```

即是 横轴竖轴都兼具都是 **20px**，也可以分开设置。

### 3.4 grid-template-areas


### 3.5 grid-auto-flow

设置 `grid` 的排序方式，默认方式 `row`，也就是

> 自动放置在每一个网格。默认的放置顺序是"先行后列"，即先填满第一行，再开始放入第二行，即下图数字的顺序。

如果修改一下即可修改为 `column` 以先列后行的方式。

`row dense` & `column dense` 意味着填充，例如

```
#container{
  display: grid;
  grid-template-columns: 100px 100px 100px;
  grid-template-rows: 100px 100px 100px;
  grid-auto-flow: row dense;
}

.item-1 {
  background-color: #ef342a;
  grid-column-start: 1;
  grid-column-end: 3;  
}

.item-2 {
  background-color: #f68f26;
  grid-column-start: 1;
  grid-column-end: 3;  
}
```

这个 `Grid` 中3行3列，且中有个占了2行。 这个时候理论上会有空隙，但是如果使用 `row dense` 或者 `column dense` 后就会自动填充后续的。

### 3.6 place-items

`place-items` 其实就是 `justify-items`，`align-items` 的合集。

`justify-items` 左中右。
`align-times` 上中下。

```
place-items: <align-items> <justify-items>;
```

```
.container {
  justify-items: start | end | center | stretch;
  align-items: start | end | center | stretch;
}
```

默认值都为 `stretch`. 自动填充。如果设置其他的无非是**上下左右居中**的区别。

### 3.7 place-content

`place-content` 也就是 `justify-content` & `align-content` 合集.

和其他一样。

```
place-content: <align-content> <justify-content>
```

接下来就是具体属性

```
.container {
  justify-content: start | end | center | stretch | space-around | space-between | space-evenly;
  align-content: start | end | center | stretch | space-around | space-between | space-evenly;  
}
```

> space-around - 每个项目两侧的间隔相等。所以，项目之间的间隔比项目与容器边框的间隔大一倍

> space-between - 项目与项目的间隔相等，项目与容器边框之间没有间隔。

> space-evenly - 项目与项目的间隔相等，项目与容器边框之间也是同样长度的间隔。

相比之前的 `place-items` 是整体的操作。

### 3.8 grid-auto-columns，grid-auto-rows

> grid-auto-columns属性和grid-auto-rows属性用来设置，浏览器自动创建的多余网格的列宽和行高。它们的写法与grid-template-columns和grid-template-rows完全相同。如果不指定这两个属性，浏览器完全根据单元格内容的大小，决定新增网格的列宽和行高。

其实就是设定超出部分的宽和高。

### 3.9 grid-[row | column]-[start | end]

> grid-column-start属性：左边框所在的垂直网格线
> grid-column-end属性：右边框所在的垂直网格线
> grid-row-start属性：上边框所在的水平网格线
> grid-row-end属性：下边框所在的水平网格线

其实也蛮简单的，就是增加一个能够扩大一个 `item` 的方式。

以水平线为基准，比如你有**三行三列**,那就有**4x4**条线。

可以使用 `span` 来表示跨越单元格，但是建议还是正常写，好明白一点。

### 3.10 grid-[column，row]

这个其实是 **3.9** 的简写

```
.item-1 {
  grid-column: 1 / 3;
  grid-row: 1 / 2;
}
/* 等同于 */
.item-1 {
  grid-column-start: 1;
  grid-column-end: 3;
  grid-row-start: 1;
  grid-row-end: 2;
}
```

### 3.11 grid-area 

设置 `grid-area` 前提是你设置过 `grid-template-areas`

```
#container{
  display: grid;
  grid-template-columns: 100px 100px 100px;
  grid-template-rows: 100px 100px 100px;
  grid-template-areas: 'a b c'
                     'd e f'
                     'g h i';
}
```

然后设置 `grid-area`.

```
grid-area: e
```

设置这个的css，就会与 `e` 中间交换。
注意 **如果设置多个会，消失。**

### 3.12 place-self

能够单独设置一个 `item` 的的对其方式

```
place-self: <align-self> <justify-self>;

.item {
  justify-self: start | end | center | stretch;
  align-self: start | end | center | stretch;
}
```

和其他一样。 以上.

### 4. 对比

`Grid` 比 `flex` 的优势来说，更适合复杂场景。

`flex` 只是对比起之前 `float` 的优化版本，如果需要写复杂的布局，同样需要大量的代码，且不移维护。

`Grid` 就很容易。且就算是单行的布局，也可以使用 Grid,也可以，所以能使用 `Grid` 我觉得就建议使用 `Grid`.

但是浏览器支持情况就很暧昧了。

`flex` 除了IE，其他浏览器的新版本基本都支持，包括个人手机浏览器，百度，qq，uc等。

`Grid` 除了IE，电脑浏览器新版本基本都支持，手机端就比较惨淡，qq，百度，opera等一众浏览器直接不支持。安卓原生浏览器也需要高版本，也就是说稍微老一点的安卓机也是不支持这个属性的.

[grid浏览器支持情况][1]
[flex浏览器支持情况][2]

### 5. end

有机会还是想在项目中试一试，比如如果有什么内部项目。

感觉以前需要蛮多css写的响应式布局，Grid很简单就能写完，这是极好的

以上.


  [1]: https://caniuse.com/#search=grid
  [2]: https://caniuse.com/#search=flex