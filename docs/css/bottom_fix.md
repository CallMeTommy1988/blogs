# 关于固定底部菜单

### 1. 使用position

```
body,html {
	padding: 0;
	margin: 0;
}

<div style="position: relative; height: 100%;"></div>
<div style="height: 60px; position: absolute; bottom: 0; background: yellow; width: 100%; clear:both;">footer</div>
```

这样就能完成在底部.

**优点**.

1. 方式简单
2. 兼容性好.

**缺点**

1. footer不能自适应
2. 父节点不能超过100%


### 2. 使用margin

```
body,html {
	padding: 0;
	margin: 0;
	height: 100%;
}

<div style="height: 100%; background: black">

</div>
<div style="height: 60px; margin-top:-60px; background: yellow">footer</div>
```

先将一个`div`的`height`为**100%**. 下面那个`div`设置为`margin-top:-60px`

**优点**

1. 方法简单
2. 兼容性好
3. 父节点可以超过100%

**缺点**

1. footer依然不能自适应(可以用js来自适应)
2. 父节点需要有明确高度(比如这里的100%,也可以直接px)

### 3. margin的变种方式

```
<div style="height: 1000px; background: black;margin: 0 auto -60px;">

</div>
<div style="height: 60px;  background: yellow"></div>
```

也是一种**margin**方式.只不过是上面`margin`和下面`margin`的区别罢了.

### 4. javascript方式

简单方式

```
<div style="height: 1000px;"></div>
<div id="footer"
      style="height: 60px;  background: yellow;  position: absolute; width: 100%; bottom: 0;"></div>
      
<script>
window.addEventListener("scroll", function() {
        $("#footer").css("bottom",- + $(window).scrollTop() + "px");
 });
</script>
```
超简单. 就固定到了底部.且就是你设置为1000px,也会固定


以上

