## 关于MVVM,MVC

要开始看看最新的前端框架，于是提前了解一下框架概念，MVVM

### 1. 什么是MVC

其实我一直不了解什么是标准的MVC. 

> model - view - controller

说法不一，实现方式也不一样.

#### 1.1 **asp.net mvc**
这里有微软的官方解释 [asp.net mvc](https://docs.microsoft.com/en-us/previous-versions/aspnet/dd381412(v=vs.108))

**models**

>    Models. Model objects are the parts of the application that implement the logic for the application's data domain. Often, model objects retrieve and store model state in a database. For example, a Product object might retrieve information from a database, operate on it, and then write updated information back to a Products table in a SQL Server database.

>    In small applications, the model is often a conceptual separation instead of a physical one. For example, if the application only reads a dataset and sends it to the view, the application does not have a physical model layer and associated classes. In that case, the dataset takes on the role of a model object.

**views**

>   Views. Views are the components that display the application's user interface (UI). Typically, this UI is created from the model data. An example would be an edit view of a Products table that displays text boxes, drop-down lists, and check boxes based on the current state of a Product object.

**Controller**

>    Controllers. Controllers are the components that handle user interaction, work with the model, and ultimately select a view to render that displays UI. In an MVC application, the view only displays information; the controller handles and responds to user input and interaction. For example, the controller handles query-string values, and passes these values to the model, which in turn might use these values to query the database.

1. models 负责数据通信，获取数据集 -> 封装到数据模型 -> 返回. 或者通过模型存储到数据库
2. views 视图是显示应用程序的用户界面（UI）的组件, 也就是 models -> views
3. controller 控制器是处理用户交互、处理模型并最终选择显示UI的视图的组件

![逻辑图](https://docs.microsoft.com/en-us/previous-versions/aspnet/images/dd381412.mvc_despattern%28vs.108%29.png)

都依赖于 **model** .

`controller` 处理用户输入 **->** `model` 来处理
`controller` 通知 `view` 更新数据
`view` 向 `model` 请求数据

#### 1.2 **Spring Web MVC**

![](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1545986910&di=e6350d3a021a13b5e16ec1d2c8207ccb&imgtype=jpg&er=1&src=http%3A%2F%2Fwww.th7.cn%2Fd%2Ffile%2Fp%2F2014%2F10%2F30%2F2a85df199ee871e2c698fdc539830a3a.png)

在拿出这一张图的时候其实我很疑惑，首先是因为我对**java**不熟悉。
其次在搜索当中大家给出的图是不一样的，我发现这种图的内容是最多的，于是以这一张作为基准.

多了一个前端控制器,把 `view` 和 `controller` 分开了. 一切交由 `dispatcherServlet` 来处理.

**浏览器** -> `dispatcherServlet` -> `controller` -> `model` -> `controller` -> `dispatcherServlet` -> `view` -> `dispatcherServlet` -> **浏览器**

不再是直接交由 `controller` 处理,降低了耦合，但是这一切都给 `dispatcherServlet` 来处理好坏我并不知道。

总之和 `asp.net mvc` 区别还是非常大的.

#### 1.3  **Rails MVC**

![](https://upload-images.jianshu.io/upload_images/3167335-85332e17ac84ff7b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/300)

这种mvc方式，其实和 `spring web mvc` 很像，不过是将 `dispatcherServlet` + `controller` 

#### 1.4 **百科中的MVC描述**

![](https://raw.githubusercontent.com/Draveness/analyze/master/contents/architecture/images/mvx/MVC-in-Wikipedia.jpg)

用户 -> controller -> model -> view -> 用户

#### 1.5 我自己理解的mvc

看了网上的一些框架的mvc的实现，mvc其实没有真正的标准。
唯一的标准就是 

view 负责ui，数据呈现。
model 负责数据查询，提供数据模型
controller 负责用户输入

仅此而已。 各种不同的框架 view可以直接请求model，也有的的要分离，减少耦合。 说白了也还就是mvc的定义模糊

### 2. mvvm
 
其实网上对于 mvvm 也有很多种说法.
view - viewmodel - model

我觉得可以简单这样理解
```
var a = { a:1,b:2 } 
$("#body").html(a.a);
```
简单来了说
`a` 就是 `model`.
`jquery` 的 **dom** 操作就是 `viewmodel`
界面上就是 `view`

这就是一种 `mvvm`
`view model` 不直接关联 交由 `viewmodel` 中转.

#### 2.1 前端的 mvvm

其实这种结构给我感觉 **viewmodel** 做的事情太多了

首先我能想到的就是一个 **监听者**, `view` 发生变化，通知 `model`. `model` 发生变化更新`view`. 但是我想每个前端框架的实现方式是不一样的。

前端的`mvvm` 主要还是双向绑定这个概念. 我想这也是能走红的原因。 真的太方便了，节约时间. 
在一个项目中变化最多的其实还是界面层，今天要加一个啥，明天要加一个啥. 有双向绑定实在是太方便了.

剩下的等我深入学习一个框架之后再来说吧


### 参考资料
- [从Script到Code Blocks、Code Behind到MVC、MVP、MVVM](https://www.cnblogs.com/indream/p/3602348.html)
- [浅谈 MVC、MVP 和 MVVM 架构模式](https://draveness.me/mvx)