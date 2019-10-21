# background-size

### 1. css2.0 api

这是之前的 `background-size` api

```
background: [<background-color>][,<background-image>][,<background-repeat>][,<background-attachment>][,<background-position>] 
```

原有的功能其实也有经常使用到。

-  background-color 背景颜色
-  background-image 背景图片
-  background-repeat 背景图片重复
-  background-position 背景图片显示位置

### 2. css3.0 api

```
background-size: auto || <length> || <percentage> || cover || contain
```

#### 2.1 length

这个比较简单

```
<div style="
        width: 500px;
        height: 500px;
        background-image: url(https://www.baidu.com/img/bd_logo1.png);
        background-repeat: no-repeat;
        background-size: 300px 50px;
        ">
</div>
```

能规定背景图片的大小。

如果只写了一个

```
background-size: 300px
```

等于

```
background-size: 300px 300px;
```

#### 2.2 percentage

其实和 `length` 是一样的。

只是可以使用百分比罢了.

不过也可以同时使用，px + %


#### 2.3 cover 

背景图片自己会放大到适合容器的尺寸

如果你是等比例的放大，其实 cover 和 contain. 是一样的

如果尺寸不一致,cover 会超出的。

#### 2.4 contain

和 `cover` 正好相反。　

他会缩放，在不超出的情况下，按比例去尽量适应这个容器



以上




