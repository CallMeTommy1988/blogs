# 关于三角形

### 基本

很多网站上都有类似对话框的三角形. 当年我做的时候,是用图片模拟的..

现在早就有很好的版本. 比如说 border.

所以说今天实践了一下.

首先是原理

```
<div style="
	display: inline-block; 
	border-top:30px solid red; 
	border-left:30px solid black; 
	border-bottom: 30px solid yellow; 
	border-right: 30px solid blue;">
</div>
```

以上代码其实可以说明一切.

这种三角形就是拼接出来的.

以这个为原理,就可以开始制作三角形

### 三角形代码

**HTML**

```
<div class="corner">
        <div class="content">
            <div class="center">
                <div class="test">

                </div>
            </div>
            <div class="center">
                <div class="test1">

                </div>
            </div>
            <div class="center">
                <div class="test2">

                </div>
            </div>
            <div class="center">
                <div class="test3">

                </div>
            </div>
        </div>
    </div>
```

**CSS**

```
.corner {
  $color: black;

  .base {
    margin: 0 auto;
    margin-top: 50px;
    width: 100px;
    height: 50px;
    background: $color;
    position: relative;
  }

  .content {
    width: 100%;
    height: 500px;

    .center {
      margin: 0 auto;
      width: 500px;
      position: relative;

      .test {
        @extend base;

        &:after {
          content: "";
          position: absolute;
          top: -10px;
          left: calc(50% - 10px);
          border-bottom: 10px solid $color;
          border-left: 10px solid transparent;
          border-right: 10px solid transparent;
        }
      }

      .test1 {
        @extend base;

        &:after {
          content: "";
          position: absolute;
          top: 50px;
          left: calc(50% - 10px);
          border-top: 10px solid $color;
          border-left: 10px solid transparent;
          border-right: 10px solid transparent;
        }
      }

      .test2 {
        @extend base;

        &:after {
          content: "";
          position: absolute;
          top: calc(50% - 10px);
          left: -10px;
          border-top: 10px solid transparent;
          border-right: 10px solid $color;
          border-bottom: 10px solid transparent;
        }
      }

      .test3 {
        @extend base;

        &:after {
          content: "";
          position: absolute;
          top: calc(50% - 10px);
          right: -10px;
          border-top: 10px solid transparent;
          border-left: 10px solid $color;
          border-bottom: 10px solid transparent;
        }
      }
    }
  }
}
```

超简单是吧.

[例子]](http://jsrun.pro/EcWKp) 

    
