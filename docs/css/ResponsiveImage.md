# 响应式图片

### 缘由

不同的尺寸其实显示不同的图片，大尺寸用大图片，小尺寸用小图片。

比如桌面网站需要精细大图，平板中号图就ok，移动端小号即可，根据用户实际可以看到的来加载图片。

现在很多响应式网站，图片用的是一套。

也就是说我在移动端也需要加载很大的图。虽然我根本用不着。

### 方案1. @media

```
@media screen and (max-width: 300px) {
    body {
        background-color:lightblue;
    }
}
```

也就是说在屏幕小于 `300px` 的情况下去改变背景。

你可以多设置几个就能够做到各种情况下的不同背景。

但是他的问题是

1. **只支持背景。**
2. 浏览器支持并没有什么优势。
3. 相对麻烦

### 方案2. srcset

```
    <img src="/static/1.jpg" style="width: 100%;"
        srcset="/static/1.jpg?id=1 300w,/static/2.jpg?id=2 600w,/static/3.jpg?id=3 900w,/static/4.jpg?id=4 1200w"
        sizes="(min-width: 1000px) 700px,100vm" alt="">
```

很简单，就是去匹配

```
srcset="路径 匹配的宽度,***"
```
以此类推。

值得注意的是，我在 **chrome** 下测试的时候，发现 **chrome** 和我想象中不太一样的地方。

如果我的初始宽度小于 `300px` 那默认显示 `1.jpg` 这个没问题，当我拖懂浏览器宽度，图片会随着我的匹配变化而变化，`1.jpg => 2.jpg => 3.jpg => 4.jpg`.

但是反过来是不行的 , 我一开始就高于 `1200px` 慢慢缩小，`chrome`  一直是 `4.jpg`。 这一点我不是很明白，**F12**, 发现有一个 `currentSrc` 属性，并且一直指向 `4.jpg`，刷新可以解决。

以下问题在firefox下没有。


**sizes**
```
图片大小 (min-width: 1000px) 设置大小 700px, 没有匹配的默认值 100vm
```

缺点:

1. 需要每一个都写，这个就很烦躁了。
2. 浏览器的兼容性一般,IE彻底无缘(不过谁叫你用IE呢)。

### 方案3. image-set

这是 **css4** 的解决方案

```
background-image: -webkit-image-set(url(http://www.baidu.com/1.png) 1x);
```

首先只能在 `background-image` 下使用。浏览器基本不支持，只能用与**chrome**私有.

### 方案4. picture

```
<picture>
    <source media="(min-width: 320px) and (max-width: 640px)" srcset="img/minpic.png">
    <source media="(min-width: 640px)" srcset="img/middle.png">
    <img src="img/picture.png" alt="this is a picture">
</picture>
```


超级好理解，他墙裂推荐的地方，他可以在自动识别屏幕方向，在特殊处理的时候更加给力

```
<picture>
    <source media="(min-width: 320px) and (max-width: 640px) and (orientation: landscape)" srcset="img/minpic_landscape.png">
    <source media="(min-width: 320px) and (max-width: 640px) and (orientation: portrait)" srcset="img/minpic_portrait.png">
    <source media="(min-width: 640px) and (orientation: landscape)" srcset="img/middlepic_landscape.png">
    <source media="(min-width: 640px) and (orientation: portrait)" srcset="img/middlepic_portrait.png">
    <img src="img/picture.png" alt="this is a picture">
</picture>
```

### 后面的疑问

作为一个伪前端，对于屏幕和分辨率一直不了解。这次本来其实是看**响应式图片**的，在查到 `srcset` 这个属性后突然发现了一个我看不懂的单位。

查询了以后才发现原来是为了适应不同屏幕下的图片。 `1x 2x`.

首先原理我就不懂. 于是查了查

[DPI、PPI、DP、PX 的详细计算方法及算法来源是什么？](https://www.zhihu.com/question/21220154)

在看了这个以后，我找到了我司的美术工程师(美工),问了问以前他是怎么做这些的屏幕的适应的。

他告诉了我，电脑端软件都是做等比缩放即可。只做一套图。

还有一种方式是根据字体来缩放。

这简直推开了我这个伪前端的新世界的大门。。以前一直没有注意这种东西，以后需要很注意。

ok，回头来说说 `1x,2x 什么意思`

我举个例子，你的屏幕是 `320px`, 你有3张图片 `1.jpg 200px 2.jpg 400px, 3.jpg 700px`;

浏览器就会这么算

> 200 / 320 = 0.625
> 400 / 320 = 1.25
> 700 / 320 = 2.1875

浏览器会根据数值自动去选择，虽然分辨率是会变化的，但是也就自适应了。

[The srcset Attribute.](https://webkit.org/demos/srcset/) 关于1x,2x的测试.

### 最后。

现在只是研究一下这个，因为后面会用到。等后面详细用到，或许会发现更多问题.

[HTML5中的picture元素响应式处理图片](https://blog.csdn.net/github_36534129/article/details/53537454)
[CSS3的srcset size属性1x 2x 3x](https://blog.csdn.net/phil_young/article/details/53729252)
[DPI、PPI、DP、PX 的详细计算方法及算法来源是什么？](https://www.zhihu.com/question/21220154)

