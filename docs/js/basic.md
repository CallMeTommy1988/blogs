# 基础类型

基础.

`javascript` 有一些基础类型. 

1. `Undefined `
2. `null`
3. `string`
4. `number`
5. `boolean`
6. `symbol`


以上有6种基础类型. 分别说说

## 1. Undefined

> Undefined 类型只有一个值，即 undefined。当声明的变量未初始化时，该变量的默认值是 undefined。

```
var o1;
let o2;
const o3;
var o4 = undefined;
function a() {
    //return;
}

function b(param1) {
    param1 == undefined;
}

o1 === undefined;
o2 === undefined;
o3 //不能这样定义,必须初始化值
o4 === undefined;
a() === undefined;
b()
```

到处都是 `undefined`, 但是只有一个值, 也很简单

```
typeof a == 'undefined'
!!a == false
```

判断 `undefined` 

`undefined` 语义在于未定义. 如果出现这个值，应该表示未定义的变量。

## 2. null

> undefined值是派生自null值的，因此ECMA-262规定对他们的相等测试要返回true

```
undefined == null //true
undefined === null //false
```

以下是判断 `null`
```
typeof null == null
!null true
```
`null` 语义在于空，比如没有查询到，没有找到，就应该返回 `null`

## 3. String

用的最多的玩意儿.

目前有3个方式
```
let str1 = "foo";
let str2 = String("foo")
let str3 = new String("foo");
```

说说区别.
> 字符串字面量 (通过单引号或双引号定义) 和 直接调用 String 方法(没有通过 new 生成字符串对象实例)的字符串都是基本字符串。JavaScript会自动将基本字符串转换为字符串对象，只有将基本字符串转化为字符串对象之后才可以使用字符串对象的方法。当基本字符串需要调用一个字符串对象才有的方法或者查询值的时候(基本字符串是没有这些方法的)，JavaScript 会自动将基本字符串转化为字符串对象并且调用相应的方法或者执行查询。

1. **字符串字面量**，也就是**基本字符串** ， "111",String("111")
2. **字符串对象** new String("111")

理论上**字符串对象**才能调用 `String` 的方法和属性. 比如 `split,indexOf`. 但是你在调用的 **字符串字面量** 的时候，`javascript` 会自动为你转换为 **字符串对象**，让你使用. 所以我们通用一般都是用 **字符串字面量**, 你直接使用**字符串对象**反而会很不方便.

```
(str1 === str2) == true
(str1 != str3) == false
(str1 === str3.valueOf()) == true
```

值得注意的是 
```
("1" == 1) == true
("1" === 1) == false //对比类型
```
原因是因为 `javascript` 的自动转换问题. 

### 3.1 属性和方法

接下来就是方法和属性。 我只看一些我没怎么用过，不是很熟悉的

#### 3.1.1 fromCharCode()  

```
String.fromCharCode(num...);
```
使用 [Unicode](https://baike.baidu.com/item/Unicode) 创建字符串, 返回**基本字符串**.

不过只支持 `number` .

#### 3.1.2 charAt

```
"a".charAt(num)
```

这个对于普通字符没有什么意义，但是一旦你文本中包含了 **Unicode** 编码就会有意义.

举个例子
```
var str = 'A \uD87E\uDC04 Z';
if(let i=0;i<str.length;i++) {
    console.log("str",i, str[i]);
    console.log("charAt",i,str.charAt(i));
}
```

当 `i = 2,3` 的时候，你直接 `str[i]` 就会乱码.
但是用 `charAt(2 or 3)` 就会返回 `\uD87E or \uDC04`.

剩下你再做处理，就可以还原对应的文字.

#### 3.1.3 codePointAt()

这个相当于返回字符串的 **Unicode** 对应编码.

举个例子
```
"啊".codePointAt(0) //21834
```
会返回对应编码。
如果你已经是编码. `"\uD87E"`.

```
"\uD87E".codePointAt(0) //55422
```
他会为你把**十六进制**转为**十进制**.

#### 3.1.4 includes

其实这个和**indexOf**是一样的, 只不过返回值不一样。

#### 3.1.5 endsWith

```
"aaa.jpg".endsWith('.jpg');
```

判断后面是否一致。 感觉能用的场景很多


#### 3.1.6 normalize

将标准的**Unicode**转为将当前字符串正规化. 也就是转为可以看得字符

#### 3.1.7 padEnd(),padStar()

```
str.padEnd(targetLength [, padString])
```
填充字符串，按照长度来重复。


## 4. number

其实 `number` 我很少用. 因为需要算的时候很少. 所以其实对于number的方法都不是太熟悉.

### 4.1 方法和属性

##### 4.1.1 EPSILON

> 两个可表示(representable)数之间的最小间隔。
> 属性表示 1 与Number可表示的大于 1 的最小的浮点数之间的差值。

很简单 1.0.0.0.0.0.0.0.........1 - 1 约等于 Number.EPSILON;

##### 4.1.2 MAX_SAFE_INTEGER，MIN_SAFE_INTEGER

> MAX_SAFE_INTEGER 常量表示在 JavaScript 中最大的安全整数（maxinum safe integer)（253 - 1）。

MIN_SAFE_INTEGER 一样,只不过是负数.

> 这里安全存储的意思是指能够准确区分两个不相同的值

```
Number.MAX_SAFE_INTEGER + 1 === Number.MAX_SAFE_INTEGER + 2 
```
这里是等于 `true` 的，也就是说你超过这个边界，那我就不保证数据准确了

##### 4.1.3 Number.MAX_VALUE ， MIN_VALUE 

这个其实是对应 `Max_Safe_Interger` & `MIN_SAFE_INTEGER`

`Max_Value` > `Max_Safe_Interger` 但是超过  `Max_Safe_Interger` 的部分保证数据准确.

`MIN_VALUE` 同理

#### 4.1.4 Number.NaN

`NaN` 相当于一个静态变量. **Not-A-Number**. 

#### 4.1.5 Number.NEGATIVE_INFINITY & Number.POSITIVE_INFINITY

正无穷大和负无穷大. 当溢出的时候就会这样，当你超过了 `max_vale`

#### 4.1.6 Number.isFinite | Number.isSafeInteger

用来检测传入的参数是否是一个有穷数

比较简单，你可以用于检测是否是数字,一个正常数字,只要是数字j就ok.

然而 `isSafeInteger` 加上了是否是安全数字的判断

比较离奇的事情

```
Number.isFinite('0') //fasle
isFinite('0') //true
```

需要统一使用风格.

#### 4.1.7 Number.toFixed

限制小数位数, 已经有会四舍五入,没有会用`0`填充.

```
var a = 1.54;
a.toFixed() //2
a.toFixed(1) //1.5
```

其实关于 `Number` 还有非常多的东西，只是我平时不太用的上，在**javascript**中需要用到这些往往涉及到动画效果，后续我需要的时候再学吧。


## 5. boolean

这个就比较简单了.

首选和其他`javascript`基础类型一样. 分为

`Boolean` & `new Boolean()` & 普通字面量

`Boolean` & 普通字面量是一样的.

```
Boolean(变量) == !!变量
if(变量) {

}
```

其实这样都一样的。
区别是在于 `new Boolean()`

```
var o = new Boolean(false)
console.log(!!o);
```

这个时候打印出来 true. 因为对象 `javascript` 会判断为 `true`

其次 `javascript` 有些特殊的写法

```
0,"",null,NaN.......
```

等等一些列的在 `javascript` 中会判断为 `false`

## 6. Symbol

这个是**ES6**中最好玩的.

### 6.1 基础用法

首先是当一个对象来用。

```
var o = Symbol("a");
var o1 = Symbol("a");

o1 == o //false
```

因为每个 `Symbol` 都是唯一的，后面的参数只不过是个描述.

```
Symbol([description])
```

描述并不影响 `Symbol` 的对比值.

首先我就想到了枚举. 加一个描述，永远不会重复，不担心有人写了一个一模一样的导致问题.

如果要查看描述的话.

```
var o = Symbol.for("a");
console.log(Symbol.keyFor(o)); //a
```

需要先注册，然后就能通过 `Symbol` 查看描述.

值得特别注意的地方是. `Symbol` 用之前的遍历方式是无法遍历出来的.

```
var a = Symbol();
var obj = {};
obj[a] = 1;
obj["b"] = 2;

for(let key in obj) {
    console.log(key);
}
// b

Object.getOwnPropertyNames(obj);
// ["b"]
```

无法遍历出 `Symbol`

有两个方法可以遍历出.
```
Object.getOwnPropertySymbols(obj); //[Symbol()]
Reflect.ownKeys(obj); //["b",Symbol()]
```

根据以上的特性，主要可以用于一些私有变量，以及一些私有属性，比如你不希望他随意被人改变。

阮一峰这里提供了一个列子
```
const FOO_KEY = Symbol.for('foo');

function A() {
  this.foo = 'hello';
}

if (!global[FOO_KEY]) {
  global[FOO_KEY] = new A();
}

module.exports = global[FOO_KEY];
```
这样就能保证一个单例，保证每次都是用的同一个实例.

### 6.2 Symbol.hasInstance

改变 `instanceof` 的值 

```
var obj = {
  [Symbol.hasInstance](obj) {
     try {
	    return !Number.isNaN(parseInt(obj));
     }
     catch(ex) {
	return false;
     }
  }
}

[] instanceof obj //false
1 instanceof obj //1
```

### 6.3 Symbol.hasInstance

> Array.prototype.concat()时，是否可以展开。

```
var a  = [1,2];
var b = [3,4];
a.concat(b); //[1,2,3,4]
b[Symbol.isConcatSpreadable] = false;
a.concat(b); //[1,2,[3,4]]
```
### 6.4 Symbol.species

> 对象的Symbol.species属性，指向一个构造函数。创建衍生对象时，会使用该属性。

这样讲，当你继承一个类的时候，其实是是用的你当前的实例，如果你需要是用到父类。就需要使用它
```
class MyArray extends Array {
  static get [Symbol.species]() { return Array; }
}
```
当你 `new MyArry` 的时候，其实是用的是 `Array`.

### 6.5 Symbol.match

> 对象的Symbol.match属性，指向一个函数。当执行str.match(myObject)时，如果该属性存在，会调用它，返回该方法的返回值。

```
var obj = {
  [Symbol.match](string) {
    return 'hello world'.indexOf(string);
  }
}
"o".match(obj); //4
"1".match(obj); //-1
```

### 6.6 Symbol.replace
### 6.7 Symbol.search
### 6.8 Symbol.split

都和 **6.5** 一样可以重写这个方法。

```
"a".replace(obj,"a") 
```
类似这样.

### 6.9 Symbol.iterator 

> 对象的Symbol.iterator属性，指向该对象的默认遍历器方法。

这个玩法很多,其实就是让你的对象按照他的方式遍历 

```
const myIterable = {};
myIterable[Symbol.iterator] = function* () {
  yield 1;
  yield 2;
  yield 3;
};

[...myIterable] // [1, 2, 3]
```

```
class Collection {
  *[Symbol.iterator]() {
    let i = 0;
    while(this[i] !== undefined) {
      yield this[i];
      ++i;
    }
  }
}

let myCollection = new Collection();
myCollection[0] = 1;
myCollection[1] = 2;

for(let value of myCollection) {
  console.log(value);
}
// 1
// 2
```

### 6.10 Symbol.toPrimitive

> 对象的Symbol.toPrimitive属性，指向一个方法。该对象被转为原始类型的值时，会调用这个方法，返回该对象对应的原始类型值

这个方法特别好玩。

```
let obj = {
  [Symbol.toPrimitive](hint) {
    switch (hint) {
      case 'number':
        return 123;
      case 'string':
        return 'str';
      case 'default':
        return 'default';
      default:
        throw new Error();
     }
   }
};
```

只能分着三种模式，按照情况来返回.

但是很不舒服的是，一般都会返回 `default`.

只能返回 `number` 或者 `string` 才会进入 `case`.

当模棱两可都可以的时候就会进入 `defaut`.