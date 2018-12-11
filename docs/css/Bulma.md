##  Bulma

### 1. 修饰符

#### 1.1 布局方式

- is-mobile 手机
- is-desktop 桌面
- is-tablet 平板

这三种决定了你在什么屏幕下怎么显示。

当你选择任何一种，意味着将为在对应的模式下，按照现在的显示，例如你选择了 `is-mobile`，本来到移动端应该平铺，但是还是会依照现在的模式显示。

这种显示模式还能有其他写法，例如说 `is-half` 这个修饰符， 可以这样写 `is-half is-mobile`. 但是可以写成 `is-half-mobile`

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

