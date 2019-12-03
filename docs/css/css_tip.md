# 纯css的tip

**HTML**

```
  <br>
  <br>
  <br>
  <br>
  <br>
  <div class="titleTips">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
    <a href="#" class="tips">
      Tips Title
      <span>Your tips description</span>
    </a>
  </div>
```

**SCSS**

```
//用于tips, 移动上去可以弹出一个小窗,不使用javascript

.titleTips {
  $icon: black;
  $border: black;
  $background: black;
  $front: #fff;

  .tips {
    position: relative;
    display: inline-block;
    text-decoration: none;
    color: #222;
    outline: none;

    &:hover span {
      visibility: visible;
    }

    span {
      position: absolute;
      width: 170px;
      margin-left: -127px;
      padding: 10px;
      border: 1px solid $border;
      opacity: 0.9;
      background-color: $background;
      text-align: center;
      border-radius: 10px;
      color: $front;
      bottom: 30px;
      visibility: hidden;

      &::before,
      &::after {
        content: "";
        position: absolute;
        z-index: 1000;
        bottom: -7px;
        left: 50%;
        margin-left: -8px;
        border-top: 8px solid $icon;
        border-left: 8px solid transparent;
        border-right: 8px solid transparent;
        border-bottom: 0;
      }
    }
  }
}

```

原理就是  `position: absolute;`.

将需要 `tip` 的文字放在 `span` 内部. 然后规定位置.

这个目前最大的问题是不能自动伸缩.

后续再考虑封装一下, 弄成一个可以配置的(不过不知道是否能纯css完成)

代码地址 http://jsrun.pro/3cWKp