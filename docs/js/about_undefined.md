# 关于undefined的杂谈

看到了一些文章的一些总结

## 最基本

首先 `type undefined` 也等于 `undefined`

和容易混淆的对象不太一样。

```
typeof null == 'object'
typeof NaN == 'number'
```

它是一个独立的对象，容易混淆的原因

```
undefined == null
!undefined == true
!!undefined == false
```

其实都和 `JavaScript` 的自动转换有关系
所以建议 `===` typescript 也是这样.

其次 `undefined` 的语义是未定义。 这点的区别也尽量明确。

## 产生undefined的常见场景


### 变量提升

```
var a;
let b;
```
关于声明变量的 `undefined` 唯一需要讲的就是变量提升

首先得讲什么是变量提升。

```
var a = 2;
```
相当于
```
var a;
a = 2
```
何为变量提升，也就是说，`var a;` 这一步会提升到上下文的顶部。

以下就是一个经典的变量提升
```
a = 2;
var a;
console.log(a);
```
其实分析下来就等于。

```
var a;
a = 2;
console.log(a);
```

所以结果是 `2`

当然 `声明 > 表达式`. 所以变量提升的优先级需要确认。

建议使用 `const` or `let`

优先 `const`. 毕竟不可改变可避免太多问题。不仅仅是变量提升，还可以解决 `for` 循环变凉作用域问题。

`let` 也行，原因是因为这两个东西在声明之前无法使用。所以完美的解决变量提升的问题。

如果是因为老式浏览器，比如`ie11`以下。 建议 `babel` or `ts` 解决问题。 

### 属性获取
```
let a = {};
a.b
a[b]

let b = [];
b[2]
```

`undefined` + `undefined` 就会报**空引用**了

我们往往都是通过`javascript` 的隐式转换去判断是否存在，其实不太科学, 也就是 

```
if(!!)
```
这种东西 `0,false.` 之类的

推荐使用，所以在获取的时候使用 `hasOwnProperty` 或者 `in`

关于这两点，其实我一直知道。

`hasOwnProperty` 自身属性 

`in` 所有属性

自身属性和所有属性。

```
if("from" in Array)
   Array.from
```

这样的判断，比直接来靠谱太多了。

### 默认值

有了默认值就能减少很多这个判断问题。

```
var aa = { }；
const { a = "defalut" } = aa；
a === "defalut";
```

这样免去了你去写三元表达式

```
var a = ("a" in aa) ? aa.a : "defalut"; 
```

理论上这样也行，但是当数量上去之后就很蛋疼。 你需要重复的写很多。使用上那一种语法会很简单。

如果再进阶一点的话就是直接在参数中使用默认值。

```
function aaaa({ a = "default" } = {}) { console.log(a) }
aaaa({ b:"a" })
//defalut
```

这样你每次都去写还是很蛋疼的。比如默认值有重复。比如多个函数需要使用，每次都写就很蛋疼了

于是这就就需要使用到 `Object.assign`

```
function aaa(options) {
    var realoptions = Object.assign({},defaultOptions,options);
}
```

或者换一种写法

```
function aaa(options) {
    var realOptions = {
        ...defalutOptions,
        ...options
    }
}
```

这里会有个疑问，因为他完全可能不传入参数

参数的默认值也应该写上.

```
function aaa(a) { 
    if(a === undefined)
        a = 2;
}
function aaa(a = 2) { }
```
如果环境支持 肯定建议第二种。


### function

如果没有 `return` 调用 `function` 会返回 `undefined`.

如果只是 `return` 不返回东西 ， 也会返回 `undefined`.

### void

## 总结

1. 善用默认值来解决 `undefined`
2. 尽量使用 `let` 或 `const` 来避免变量提升或者未赋值的情况
3. 变量的生命周期尽量缩短，别动不动就来全局变量
4. 正确的验证方式 `in` 来验证属性

以上
