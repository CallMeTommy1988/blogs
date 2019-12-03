# 纯css的tab

其实这个东西我一开始是没有想明白的.

后来看了 w3cpuls.com 给出了答案.

以下是我理解了思路之后, 自己重写的

**HTML**

```
    <div class="css_tab">
      <div class="cssTabs">
        <div id="tab1" class="uiTab">
          <h3 class="tabHead">Header for Tab1</h3>
          <div class="tabContent">Content for Tab1</div>
        </div>

        <div id="tab2" class="uiTab">
          <h3 class="tabHead">Header for Tab2</h3>
          <div class="tabContent">Content for Tab2</div>
        </div>

        <div id="tab3" class="uiTab">
          <h3 class="tabHead">Header for Tab3</h3>
          <div class="tabContent">Content for Tab3</div>
        </div>
      </div>
    </div>
```

首先要纯css实现,就必须按照上面的实现.

需要 `uiTab` 完成 `hover` 来显示内容. 如果按照 **javascript** 的做法分割是无法实现的

**SCSS**

```
@mixin css_tab_left($index) {
  left: $index * 178 + px;
}

.css_tab {
  background: white;

  .cssTabs {
    position: relative;
    width: 600px;
    height: 290px;
    margin: 0 auto;
    background: white;

    .uiTab {
      .tabHead {
        position: absolute;
        padding: 9px;
        border: 1px solid black;
        border-top-left-radius: 10px;
        border-top-right-radius: 10px;
        z-index: 3;
        background: white;
        margin: 0;
      }

      .tabContent {
        position: absolute;
        top: 44px;
        border: 1px solid black;
        height: 230px;
        width: 100%;
        background: white;
        z-index: 2;
        visibility: hidden;
        padding: 10px;
      }
    }

    #tab1 {
      .tabHead {
        border-bottom: none;
        padding-bottom: 10px;
      }
    }

    #tab2,
    #tab3 {
      &:hover {
        .tabHead {
          border-bottom: none;
          padding-bottom: 10px;
          z-index: 5;
        }

        .tabContent {
          visibility: visible;
          z-index: 4;
        }
      }
    }

    #tab1 {
      .tabContent {
        visibility: visible;
      }
    }

    #tab2 {
      .tabHead {
        @include css_tab_left(1);
      }
    }

    #tab3 {
      .tabHead {
        @include css_tab_left(2);
      }
    }
  }
}
```

全部绝对定位,以及高度固定. 就可以实现了.

代码地址 http://jsrun.pro/z7WKp



