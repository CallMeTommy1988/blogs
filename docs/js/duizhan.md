# 关于执行上下文和调用栈

首先明确
> js 是单线程. 

除了 `webworker` 其他都是单线程. 也就是说那些异步都只不过之一种在**单线程**下的一种处理方式,与多线程无关.

## 1. 执行上下文

我们需要大概知道他界面是如果创建出来的.
首先在创建的过程中会发生三件事情.

>    this 值的决定，即我们所熟知的 This 绑定。
>    创建词法环境组件。
>    创建变量环境组件。

`this` 很好理解. 就是绑定当前的 `this` 指向
**创建词法环境组件** 和 **创建变量环境组件** 是相对的。
变量意味着 var
词法意味着 function,let,const 这些

在**词法环境**中，下面有 **环境记录器** 这样一个东西，用于存储你绑定的值, **变量环境**同样.

```
let a = 20;
const b = 30;
var c;

function multiply(e, f) {
 var g = 20;
 return e * f * g;
}

c = multiply(20, 30);
```

```
GlobalExectionContext = {

  ThisBinding: <Global Object>, //绑定global

  LexicalEnvironment: { //词法环境
    EnvironmentRecord: { //环境记录器
      Type: "Object",
      // 在这里绑定标识符
      a: < uninitialized >, //声明但是没有绑定
      b: < uninitialized >, 
      multiply: < func > //function 直接解析
    }
    outer: <null> //外部环境为null
  },

  VariableEnvironment: {
    EnvironmentRecord: {
      Type: "Object",
      // 在这里绑定标识符
      c: undefined,
    }
    outer: <null>
  }
}

FunctionExectionContext = { //multiply
  ThisBinding: <Global Object>,

  LexicalEnvironment: {
    EnvironmentRecord: {
      Type: "Declarative",
      // 在这里绑定标识符
      Arguments: {0: 20, 1: 30, length: 2}, //参数
    },
    outer: <GlobalLexicalEnvironment> //外部global
  },

VariableEnvironment: {
    EnvironmentRecord: {
      Type: "Declarative",
      // 在这里绑定标识符
      g: undefined
    },
    outer: <GlobalLexicalEnvironment>
  }
}
```
这里是网上的一个列子,var 居然是单独声明的哈哈哈


## 2. 调用栈

js很简单，栈就是一个后进先出的队列。引用网上的图

![](https://user-gold-cdn.xitu.io/2017/11/11/bc37a6231fca3b0aa3cd36369e866837?imageView2/0/w/1280/h/960/ignore-error/1)

```
    function multiply(x, y) {
      return x * y;
    }
    function printSquare(x) {
      var s = multiply(x, x);
      console.log(s);
    }
    printSquare(5);
```

1. `printSquare(5)`
2. `printSquare(5)` -> `multiply(x, x);`
3. `printSquare(5)`
4. **global**

这就是内部的机制。 **后进先出**, 其实一般的也是这样。

因为这样的机制，所以理论上不允许你无限的去增加栈的层级。
```
var i = 0;
function a() {
  i++;
    arguments.callee();
}
a();
```
> InternalError: too much recursion

栈溢出，超过最大的层级.

```
i = 11242
```

在 **firefox** 中最大层级也就是这么大了，每个浏览器是不太一样，我测试 **chrome** 就会大一些.

这是我们口中的描述，如何印证这一点呢？
```
   function multiply(x, y) {
     console.trace();
      return x * y;
    }
    function printSquare(x) {
      console.trace();
      var s = multiply(x, x);

    }
    console.trace();
    printSquare(5);
```
输入 **栈** 信息.

```
console.trace() debugger eval code:10:6
<匿名> debugger eval code:10:6

console.trace() debugger eval code:6:7
printSquare debugger eval code:6:7 
<匿名> debugger eval code:11:5

console.trace() debugger eval code:2:6
multiply debugger eval code:2:6 printSquare 
debugger eval code:7:15 
<匿名> debugger eval code:11:5 
```

三个地方调用结果一目了然.

这就是`js`调用栈的最基本的机制

## 关于异步.

### 基础

其实上面讲的是正常调用，还有一些执行没有讲到.
例如 `settimeout`，`Promise` 等等. 是如何计算这些步骤的执行的.

这里来自于网上的一幅图
![](https://user-gold-cdn.xitu.io/2017/11/21/15fdd88994142347?imageView2/0/w/1280/h/960/ignore-error/1)


>    ajax进入Event Table，注册回调函数success。
>    执行console.log('代码执行结束')。
>    ajax事件完成，回调函数success进入Event Queue。
>    主线程从Event Queue读取回调函数success并执行。

也就是说异步是另一种场景，他会在主线程完成后再执行. 不管你`settimeout`设置的是多少**ms**,都将晚于主线程正常执行.

```
setTimeout(function() { console.log(1) },0)
console.log(0)
```

也会是 **0** 先执行.

其次 `Promise` 的前段是同步执行的

```
new Promise(function(resolve) {
    console.log('4');
    resolve();
}).then(function() {
    console.log('5')
});
```

**4** 会历经执行，也就是走的主线程。同步任务. 只有`then` 才会走异步流程.

### 宏任务，微任务

在浏览器里不仅仅只有异步和同步这一个概念.
还有宏任务和微任务两个概念.

**微任务** `Promise`，`process.nextTick`
**宏任务** 其他应该都算宏任务.

解释是这样的.

宏任务 -> 微任务 - event-loop无限循环

```
setTimeout(function() {
    console.log(2);
},4);

new Promise(function(resolve) {
    console.log('4');
    resolve();
}).then(function() {
    console.log('5')
});

console.log(1);
```

`setTimetout` 异步任务， 加入到异步的步骤中，属于宏任务.
`new Promise` 开始同步任务, 立即执行 **4**, `then` 加入到微任务当中
`console.log(1)` 同步任务，直接执行 **1**

当第一轮的宏任务执行完成。 执行微任务
`then` 输出  **5** .

第二轮宏任务. `setTimeout callback` 执行 **2**.

## 结束.

这就是调用栈和异步的执行方式.
