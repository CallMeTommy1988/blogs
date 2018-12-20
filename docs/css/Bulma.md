##  Bulma

### 1. 修饰符

#### 1.1 布局方式

- is-mobile 手机
- is-desktop 桌面
- is-tablet 平板

这三种决定了你在什么屏幕下怎么显示。

当你选择任何一种，意味着将为在对应的模式下，按照现在的显示，例如你选择了 `is-mobile`，本来到移动端应该平铺，但是还是会依照现在的模式显示。

这种显示模式还能有其他写法，例如说 `is-half` 这个修饰符， 可以这样写 `is-half is-mobile`. 但是可以写成 `is-half-mobile`

- is-fullhd 全屏
- is-widescreen 宽屏

### 2. 列 columns

语义上定义为列.

所以类似与表格上的列，也可以是 `ul li` 或者 `ol li`，但是这个列是一个由 `div` 组成的响应式布局。

#### 2.1 普通的列

```
<div class="columns">
  <div class="column">
    第一列
  </div>
  <div class="column">
    第二列
  </div>
  <div class="column">
    第三列
  </div>
  <div class="column">
    第四列
  </div>
</div>
```
![](/images/1.png)
会平均分配，这在这个基础上

为了方便你控制大小，它提供了2种方式让你来控制大小。

#### 2.2 列的大小

##### 2.2.1 is-number

这是一种基础的方式.

![](/images/2018-12-1123-18-30.png)

你其他的任何元素都可以通过 `is-number` Bulma

##### 2.2.2 is-number-number

这里的 `number` 是英文的数字,他的计算方式就是 `number/number` 来计算几分之几.

1. is-three-quarters
2. is-two-thirds
3. is-half
4. is-one-third
5. is-one-quarter
6. is-four-fifths

也就是这样的算法.

##### 2.2.3 Offset

可以设置偏移.

1. `is-offset-number`
2. `is-offset-number-number`

同样也是分两种，第一种 `number` 就是 1，2，3，4. 离开左边偏移
第二种计算方式就是 `number/number` 来计算几分之几.

#### 2.3 列的嵌套

只要满足了下面的条件就可以了

> columns:顶层列
>   column
>       columns: 嵌套列
>           column 更多...

理论上是可以无限嵌套

```
<div class="columns">
    <div class="column">
        <div class="columns">
            <div class="column">
                <p class="notification is-info">第一列</p>
            </div>
            <div class="column">
                <p class="notification is-success">第二列</p>
            </div>
            <div class="column">
                <p class="notification is-warning">第三列</p>
            </div>
            <div class="column">
                <p class="notification is-danger">第四列</p>
            </div>
        </div>
    </div>
    <div class="column">
        <p class="notification is-success">第二列</p>
    </div>
    <div class="column">
        <p class="notification is-warning">第三列</p>
    </div>
    <div class="column">
        <p class="notification is-danger">第四列</p>
    </div>
</div>
```
![](/images/2018-12-1123-56-50.png)

#### 2.4 gap 列间距

列之间有间距，间距有3种修改方式

##### 2.4.1 $column-gap

这种方式通过，重载变量的方式. 前提你得使用 `sass`

##### 2.4.2 is-gapless

取消间距

##### 2.4.3 is-variable is-number

```
class="columns is-variable is-2"
```
然后通过 `is-(0-12)` 的数字来设置间距

##### is-multiline

设置 `columns` 不换行.

##### is-centered

设置 `columns` 居中

### 3. 容器

#### 3.1 基础容器

`container` 他会其起一个居中作用，两边留有空隙 

```
<div class="container">
  <div class="notification">
    This container is <strong>fluid</strong>: it will have a 32px gap on either side, on any
    viewport size.
  </div>
</div>
```

> 在 $desktop 下，容器最大的宽度是 960px.
> 在 $widescreen 下，容器最大的宽度是 1152px.
> 在 $fullhd 下，容器最大的宽度是 1344px.

以上就是这个容器的最大宽度。 如果你不想要两边的边距，可以使用 `is-fullhd & is-widescreen` 来填充满。

#### 3.2 level

也算是一种基础容器，应该多用于导航。

```
<nav class="level">
    <div class="level-left">
        <div class="level-item">
            <p class="subtitle is-5">
                <strong>123</strong> posts
            </p>
        </div>
    </div>
    <div class="level-right">
        <p class="level-item"></p>
    </div>
</nav>
```

能够创建出一个类似下面这种的结构，一左一又。

![](/images/2018-12-1223-35-24.png)

可用于导航，或者这种需要. 也应该主要用于这种。 文档中还介绍了一种方式

```
level -> level-item
```

省掉了中间这一个级，会平均分配.

#### 3.3 Media Object

媒体资源.

他提供一种 左中右的结构,

左侧信息，中间是内容，右侧可以放一些操作或者信息。

```
media -> 
    media-left
    media-content
    media-right
```

![](/images/20181220220036.png)

#### 3.4 Hero

> 一个用来展现一些壮观事物的 英雄横幅

可用于导航，以及展示页的画卷    

![](/images/2018-12-2022-42-12.png)

```
hero ->
    hero-head
    hero-body
    hero-foot
```

head 也可以加入导航，一些按钮。foot同理.

### 4. 小结

这个css框架基于 `flex`

该有的都有，只是可能没有 `bootstrap` 插件那么多

单独写只提供一些基本的布局，然后你写一些修饰符，来限制一系列的东西。

通过修饰符，以及一些基本布局，能让你写网站更加方便。

如果要根据美工的图片的大小，来布局，你就需要使用sass来重载。

如果只是想要简单建站，就直接使用组件。