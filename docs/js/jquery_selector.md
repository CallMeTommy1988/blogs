# 关于Jquery选择器

其实无非就是Jquery的选择器和原生选择器.

这次先看了Jquery 选择器，其实在很早就在用了，只是没有系统了总结一下。

以上的实例

### 1. 基础的选择

#### ID选择 #

```
$("#xxxx")
```

这个不会有人不知道吧。

#### class .

```
$(".className")
```

#### 标签集合 tagName

这种其实在第一级用的比较少。

```
$("div") 
```
所有 `div` 都被找到了。

#### * 所有元素 

```
$("*")
```

### 2.层次选择

在上面基础选择的基础上，可以做层次选择

#### 后代元素 white space

```
$("ancestor  descendant")
```

包含所有后代元素，也就是说 `ancestor` 下面所有元素，都算在内，不管你下面有多少层。

#### 子元素选择器 >

上面后代选择器，相当于说子子孙孙都选了。 这个只选儿子

```
$("parent > child") 
```

一个比较简单的例子，打开百度 **f12**

```
$("form input").length	
```
**14个** input

换成子元素的话

```
$("form > input").length	
```

**12个** input

#### 相邻元素选择器 + 选择面的最靠近的一个

其实这种选择器我是今天才知道的，他只能选定相邻选择器，且只能选择下级。	

```
$("prev  +  next") 
$(".soutu-btn + input").length
```

为什么没有用到，因为一般会这样使用

```
$(".soutu-btn").next();
```

也能达到同样的效果，且都要判断一下，只不过 用选择器不用去判断标签。


#### 兄弟选择器 ~ 选择后面所有的兄弟

后去后续所有的节点。

```
$(".soutu-btn + input").length
```

之前的 **相邻元素选择器** 只是选择下面的兄弟。这个就是所有的下面兄弟。

```
$("prev ~  siblings")
$("#s_kw_wrap ~ input")
```

下面所有的 **12个** `input` 全部选择了。

这个其实也没有用过，因为也有替代方法

```
$("#s_kw_wrap").nextAll("input")
```

### 3. 过滤

#### 基本过滤器，选取第一个元素 :first

```
$('E:first')
```

选取第一个元素。 

```
$("form input:first")
```

选取 整个页面上 `form` 的第一个 `input`.

唯一需要知道的是，他的第一个是不分层次的，就是 `form` 层次中就是最前的 `input`. 

#### 基本过滤器，选取最后一个元素 :last

和上面一样，只不过他选最后一个

#### 基本过滤器 :not(selector)

```
$("E:not(selector)")
```

就是在选择的同时，增加一种排除条件。

```
$("input").length
```

有 **15个** input。

```
$("input:not(form input)").length
```	

只有 **1个** input. 排除了 	`form input`。

#### 偶数选择器 even

```
$("E:even")
```

偶数索引。

```
$('form input:even').length
```

偶数索引。返回**7个**

#### 奇数选择器 :odd

```
$("E:odd")
```
奇数索引，能够返回奇数 `1,3,5,7,9`

```
$('form input:odd').length
```

返回**7个**奇数索引

#### 根据索引选择 :eq

```
$("E:eq(index)")
```

`index` 从0开始。 其实就可以模拟 `last` & `first`

```
$("form input:eq(0)")
$("form input:first")
```

两个结果是一样的。

#### 根据大于索引返回集合 :gt

```
$("E:gt(index)")
```

比如说 有 **14个** `input`，

```
$("form input:gt(10)")
```

由于是从 **0** 开始的，所以 `14 - 11` 会返回3个

#### 根据小于索引返回集合 :lt

```
$("E:lt(index)")
```
从0开始计算索引。

```
$("form input:lt(10)")
```

返回**小于10**的索引，也就会**10**

#### Header 选择器 :header

``` 
$(":header")
```

能够选择 `h1 - h6` 的元素。

百度首页居然没有 `h元素`

于是随便搜索了一个页面。

```
$(":header")
```
返回了 **12个** 元素.

```
$("#1 :header")
```

返回当前元素下的 `h1-h6`。

### 4. 文本选择器

#### 包含文本 :contains

这个挺厉害的，感觉在动态文本的查询上面很有用处。

```
$('a:contains(新闻)')
```

就能返回百度首页的**a链接xinwen **

#### 空文本 :empty

```
E:empty
```

返回没有文字的空文本节点。

```
$(":empty") 
$("div:empty")
```
一个返回所有的空文本节点，一个返回 div 下所有空文本节点.

#### 包含选择器 :has(selector) 

```
:has(selector)
```

包含的选择器

```
$("div:has(p)")
```

返回的不是`p` ,而是 `div`，包含有`p`的 `div`.

#### 子元素选择器 :parent

```
E:parent
```

判断一个节点是否有子元素，如果有就返回`E`

```
$(".soutu-btn:parent")
$("#s_kw_wrap:parent")
```

第一个没有子元素，返回空，第二有子元素，返回本身


### 5. 结尾

其实都不太难，很容易理解。只是一般都是边用边差，从来没有系统的知道一下，所以有一些挺好用的选择器其实一直没有使用过。

后续如果还能想的起的话就能经常使用了
