# Border-radius

### 1. 关于Border-radius

其实圆角边框一直都在用，伪前端不清楚具体的接口。

### 2. 浏览器支持

[Border-radius 浏览器支持](https://caniuse.com/#search=border-radius)

基本上所有浏览器都支持，除了 **IE8 及其以下浏览器**

我基本已经不用去考虑兼容 **IE8**，所以 **IE8** 不用考虑。

### 3. API

####  3.1 border-x-x-radius

首先得知道，有4个脚。

```
border-top-left-radius: <length>  <length>   //左上角
border-top-right-radius: <length>  <length>  //右上角
border-bottom-right-radius:<length>  <length>  //右下角
border-bottom-left-radius:<length>  <length>   //左下角
```
比如边框的左上角

```
border-top-left-radius: <length>  <length>   //左上角
```

有两个参数，区分 **水平参数**和**垂直参数**。

```
width: 100px;
height: 100px;
background: radial-gradient(red, black);
border-top-left-radius: 100px 100px;
```
是一个三角形的扇形

```
width: 100px;
height: 100px;
background: radial-gradient(red, black);
border-top-left-radius: 100px 50px;
```

这样更明显。

一个垂直，一个水平。

其他三个角也一样。

如果你垂直和水平都是一样的大小

```
border-top-left-radius: 100px;
border-top-left-radius: 100px 100px;
```

是一样的。



#### 3.2  border-radius


```
border-radius ： none | <length>{1,4} [/ <length>{1,4} ]?
border-radius: 50px;
```

这样写等于 

```
border-top-left-radius: 50px;  //左上角
border-top-right-radius: 50px;  //右上角
border-bottom-right-radius:50px;  //右下角
border-bottom-left-radius:50px;   //左下角
```

`border-radius` 是之前那些写法的简写。

这种简写如果 **水平和垂直** 不一致怎么办?

```
border-radius: 50px / 40px;
border-top-left-radius: 50px 40px;  //左上角
border-top-right-radius: 50px 40px;  //右上角
border-bottom-right-radius:50px 40px;  //右下角
border-bottom-left-radius:50px 40px;   //左下角
```

可是依然会有问题，如果每一个角都不一样怎么办？

```
border-radius: 50px 40px 30px 20px;
```

等于

```
border-top-left-radius: 50px; //左上角
border-top-right-radius: 40px; //右上角
border-bottom-right-radius: 30px; //右下角
border-bottom-left-radius: 20px; //左下角
```

最后是在所有都不一样的情况，每个都角都不一样，**水平和垂直**也都不一样

```
border-radius: 50px 40px 30px 20px / 20px 30px 40px 50px;
```

等于

```
border-top-left-radius: 50px 20px; //左上角
border-top-right-radius: 40px 30px; //右上角
border-bottom-right-radius: 30px 40px; //右下角
border-bottom-left-radius: 20px 50px; //左下角
```

### 4. border 与 border-radius 关系

```
border: 15px solid green;
border-radius: 15px;
width: 100px;
height: 100px;
background: radial-gradient(red, black);
```

你会发现，外层的边框是有圆角的。但是内存的实际内容是没有的。

其实他们的关系是 `border-radius大小 - boreder大小` 就能得到里面内容的圆角

```
border-radius: 21px;
border: 15px solid green;

border-radius: 7px;
border: 1px solid green;
```

以上



