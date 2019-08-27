# 关于RGBA

### 关于RGB的常识

其实经常使用这个东西，但是作为一个伪前端，其实一直不知道这玩意儿是什么意思，就知道是个颜色。

#### 1.1 什么是RGB

- R：红色值。正整数 | 百分数
- G：绿色值。正整数 | 百分数
- B：蓝色值。正整数| 百分数

```
rgba(255, 255, 0)
```

这三个参数就是我们常看到的`RGB`

**红、绿、青** 是三原色, 他可以组成任何其他颜色。

但是我们这里用的是 **红绿蓝** 

蓝色是 品红和青混合的颜色。

**红绿蓝** 被称为三基色. 

[百度百科三基色](https://baike.baidu.com/item/%E4%B8%89%E5%9F%BA%E8%89%B2/4469272?fr=aladdin)

我的理解是 **三基色** 这个标准几乎包括了人类视力所能感知的所有颜色，只不过和三原色的应用场景不同罢了。

#### 1.2 十六进制颜色

其实就是 **RGB** 专程 16进制

### 2. RGBA

#### 2.1 RGBA与RGB的区别

其实其他都一样就是多了一个透明度选项

```
<opacity> ：alpha(透明度)。 取值在0到1之间；
```

**A = alpha**

可以在写

```
rgba(255, 255, 0, 0.2)
```

第四个参数上面

#### 2.2 opacity

在 **css2** 当中也有使用过透明，

```
opacity: 0.2;
filter: alpha(opacity=20);
-ms-filter:"progid:DXImageTransform.Microsoft.Alpha(Opacity=20)"; 
```

`filter` 是用于兼容IE8及以下的方案

#### 2.3 RGBA和opacity区别

理论上来说

```
opacity: 0.2;
background: rgb(255, 255, 0)
```

就可以和 `RGBA` 有一样的效果，而且效果真的一样啊。

只是差别在于子集。

```
opacity: 0.2;
```

不管你想不想下层透明，你都得透明，如果不想透明，就得使用专门的写法。

```
//原本
<div class="bg-box">
    <div class="bg">
        <div class="bg-content">
            <p>a</p>
        </div>
    </div>
</div>

//子节点不透明
<div class="bg-box">
    <div class="bg">
    </div>
    <div class="bg-content">
        <p>a</p>
   </div>
</div>
```

采用 `bg-box` 就只能 `position: relative;`，下面两个子元素采用 `    position: absolute;` 才能实现。

但是 `RGBA` 原本就可以，不用去改变DOM结构

#### 2.4 场景

任何使用颜色的地方都可以使用 `RGBA`。

当然我没有全部测试，**背景，阴影，边框，字体颜色**

以上





