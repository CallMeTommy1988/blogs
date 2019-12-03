# 兼容性的小知识

### 1. base

其实浏览器有点大一统的意思了.

IE大部分网站仅仅兼容到IE11. 可能未来IE11也会放弃掉.

edge 选择了 Chromium Edge

opera 已经转为了 Chromium. 且市场占用极少.

firefox 在国内除了程序员没人用, 国外市场还是需要考虑 firefox.

目前按照我司的要求

1. IE10+
2. firefox
3. chrome
4. edge

兼容以上4种浏览器即可. 

如果你做到了基本上可以兼容大部分其他浏览器

**IE10+** 和 **edge** 提供基本功能. 新技术应用于 firefox 和 chrome



### 2. 关于IE浏览器的选择

IE浏览器生效

```
<!--[if IE]>
	<link rel="stylesheet" href="all-ie-only.css"  type="text/css"/>
<![endif]-->
```

非IE浏览器生效

```
<!--[if !IE]>
	<link rel="stylesheet" href="not-ie.css"  type="text/css"/>
<![endif]-->
```

仅支持某个版本的IE

```
<!--[if IE n]>
    <link rel="stylesheet" type="text/css" href="ie.css">
<![endif]-->
```

仅支持某个版本一下的IE, 比如下面是 IE10 以下版本.

```
<!--[if lt IE 10]>
    <link rel="stylesheet" type="text/css" href="ie9-and-down.css">
<![endif]-->
```

高于某个版本的IE

```
<!--[if gt IE 9]>
    <link rel="stylesheet" type="text/css" href="ie10-and-up.css">
<![endif]-->
```

### 3. hack

##### 3.1 firefox hack

firefox 专属写法

```
@-moz-document url-prefix() {
   .demo {
     background: red;
  }
}

//这种方式兼容Gecko内核的浏览器
*>.selector { property: value; }

#selector[id=selector] { property: value; }
```

直接写在里面即可.  

只会在**firefox**生效.

资料中说的 

```
*>.selector { property: value; }
```
只会在 `firefox` 中生效, 但是经过我的测试在 `chrome` 中也会生效.

只有第一种方式,才是只会在 **firefox** 中生效.

##### 3.2 webkit 浏览器写法(chrome,sarfai)

```
@media screen and (-webkit-min-device-pixel-ratio:0)	{
	.demo { background:red }
}
```	
经过测试之后,最新版本 **firefox** 中也支持.

在查过之后, 不仅仅是 **firefox** , **Edge** 也是支持的

[-webkit-min-device-pixel-ratio](https://caniuse.com/#search=-webkit-min-device-pixel-ratio)

### 4. 总结

优先兼容 `webkit`.

其次兼容 **IE10+** 和 **firefox**.

不行就用hack



