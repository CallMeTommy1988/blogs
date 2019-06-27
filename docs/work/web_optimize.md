# 网站优化

公司网站的基本开发已经完成了，接下来就要开始优化性能。

首先优化论坛首页

## 1. 分析

打开Chrome -> F12 -> Disable cache -> 访问网站

## 1.1 基本数据

|整体速度|下载大小|请求数量|XHR|
|-|-|-|-|-|-|
|10.25s|3.6mb|33|none|
|9.66s|3.6mb|33|none|
|9.95s|3.6mb|33|none|
|9.95s|3.6mb|33|none|
|10.53s|3.6mb|33|none|
|10.08s|3.6mb|33|none|
|10.28s|3.6mb|33|none|
|10.23s|3.6mb|33|none|

速度大概在这个区间之内

**CSS**
首页目前只加载1个css `layout.css`. 大小143kb. 速度在**300** - **450** ms左右.

**image**
总共23个请求. 总大小**1.8mb**
最大图片 **1.6mb** 平均需要**9s**
板块封面图6个 
导航图片1个
其他是零散的图片

**js**
总用**8**个请求，总大小 **1.6mb**
特别注意的是 需要等verdor.js -> 0.js -> 2.js & 3.js

### 1.2 图片分析

首先可以整合两种图片
1. 头像图片
2. 零散的图标
3. 板块图片
4. 板块,首页背景图片
5. 用户上传图片

**头像合并，零散图片合并。** 

这两个是首要且目标明确的优化,合并，并且使用css精灵组成一张图片减少请求数量. 并且压缩图片

**板块图片以及板块首页的背景图片**

问题是图片过大需要压缩,且需要动态判断是否需要合并。如果需要合并，还需要生成css.

**用户上传图片**

用户在编辑器中生成的图片，我们无法动态插入html（其实是可以的，不过要修改编辑器，并且需要同步webp，jpg之类）.

所以这一端只需要做压缩图片的功能


### 1.3 脚本分析

总用**8**个请求，总大小 **1.6mb**

首先请求数量过多。 8个，且有依赖。会造成速度请求过慢

```
vendor.js -> main.js -> 0.js -> 2.js & 3.js
```

不能并行。
且有过大的文件，过小的文件，需要综合.

需要减少请求数量，以及压缩大小.

## 2. 解决

### 2.1. 合并图片资源

#### 2.1.1 合并做法

本来打算直接使用别人的 `css sprite` 但是后来发现不行，原因是网站之前的图片并不是严丝合缝的。

也就是说，比如一个背景图片

```
background:url('../../img'); no-repeat center;
```
也就是说图片大小和背景图片的容器不一致。 于是生成的css，经常会出现在容器的顶部。 需要重新调整 `position` 的位置.

于是自己写了一个生成 `css sprite` 的脚本，可以传入 `function` 可以重新校正 `position`.

然后重新替换了所有图片。但是增加了一个 `web.css` 增加了一个请求.

这样解决了头像问题.

然后找到了所有的工具图片，零零碎碎的小图片，然后生成css以及图片，讲图片合并。

#### 2.1.2 大小不一致的问题

在做完这个以后，发觉这个解决方案的弊端，就是大小无法改变，只能撑满。在我们这边项目当中特别麻烦。

```
div { width:50px; height:50px }
img { width:30px: height:30px }

background-position: center
```

这是外包的伪代码。他这样写，使用css精灵这种做法，就很难去做，必须扩大背景，且为这种地方写专门的特殊代码。

#### 2.1.3 动态变化问题。

还有一种不能解决的问题，就是父层的变化，当浏览器的高宽的变化以后，无法自适应。例如板块图片，忽大忽小背景图很难做.

这个问题导致了目前部分工具图片只能保证在宽屏下使用，如果在小屏幕下需要重新下载小屏幕图片.

### 2.2 压缩图片

需要引用库去压缩图片，生成图片.

压缩图片分2种。

首先是正常压缩，用户上传时候自动压缩图片，只是需要知道用户上传后是否可以合并以及生成webp格式。

目前就是纠结于是监控文件夹变化去做，还是在后台上传图片去做。

我这边建议是监控文件夹去做。功能拆分出来，更稳定一点。

### 2.3 优化脚本.

首先是量太大了。 **总用8个请求，总大小 1.6mb**.

**1.6mb** 实在是太大了，而且**仅仅是首页**，于是优化.

**首先是优化大小**。

1. 由于之前论坛是由开发者社区改进过来的，有其他同事以前写过的现在不用的代码, 所以**清理代码**。
2. 压缩，因为之前代码没有经过压缩，压缩.
3. 清理使用不多且大的npm包. 

在清理的完成后，包大大减小。


**其次所以减小请求数量**

但是问题还是有.

```
vendor.js -> main.js -> 0.js -> 2.js & 3.js

459ms -> 1.6s -> 1.81s -> 1.90s
```

并没有并行下载，很影响速度，但是又有依赖关系，如果直接合并不太好，因为太大了，超过 `100kb`

加上 一些外部JS 3个 总共有8个js。

首先去掉 Vender.js . 在取消 `moment.js` 后已经很小了，于是取消掉。减少一个请求。 目前减少到7个请求.

其次首页不需要这么多的异步。于是减少 `JSONP` 异步请求, 保证每个页面除了加载 `main.js` 以外，最多加载一个js请求。也就是保证页面最多一次性加载 **5个js**。

减少以后每个请求只有5个请求，但是有些碎片，有些页面是这样的

```
main.js -> 0.js
```

`0.js` 只有 **4.5k** ，所以把小`JS`合并进入其他小的**JSONP**加载的JS当中。

所以现在目前生成了6个**JS**.

每个页面都会加载 `main.js` 其他页面按需加载，比如首页就只需要 4个js

**首页**
```
jquery.min.js 95.3kb
swiper.jquery.min.js 85.3kb
layout.js 20.7kb
main.js 298kb
```

大概接近**500kb**的样子,已经很少了,减少了之前的四分之三，且也减少了请求数量,也保证了充分利用缓存。

但是也有继续优化的潜力。

1. 首先 **layout.js** 并没有压缩。能减少一部分。
2. **layout.js** 太小了，且 `jquery` & `layout` 每个页面都会加载，可以合并起来。
3. 拆分 **main.js** 一个大的 `main.js` 有300kb,可以对半拆分增加并行的效率

在尝试拆分的时候，发现 `package.json` 引用过于混乱，需要先整理 `package.json` ，并且js和后端在同一个项目，导致的引用混乱。 后续优化应该先考虑分开项目再分别优化,目前就不在这一次的优化考虑当中.

## 结果

### 1. 脚本

脚本从每个页面7-8个js。总量维持在1.5mb+ 更新到 每个页面维持在3个，总量维持在450kb左右，且充分利用缓存,减少请求.

### 2. 图片

器本地测试的结果，能将各种导航图，以及封面图压缩，70%的质量，能压缩6倍左右，且图片没有明显的变化，例如颜色丢失之类的，只有在详细两张图对比的情况下，才会看出些许差别。

速度有明显提升但是肯定没有6倍，毕竟现在的带宽。
理论上进一步优化应该合并图片，只是可惜外包提供的页面并不支持这种优化。

这部分还没有上服务,因为负责管理这边的同事觉得压缩图片风险太大。sad

