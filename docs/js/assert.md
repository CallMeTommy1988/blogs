# assert

我主要用 `Nodejs` & `javascript`

所以需要看这两个的 `assert`.

### nodejs assert

其实类似单元测试，用于推断值是否正确。 只是可能没有单元测试覆盖那么广。

#### 1. AssertionError
```
 options <Object>
 
 message <string> 如果提供，则将错误消息设置为此值。
 actual <any> 错误实例上的 actual 属性将包含此值。在内部用于 actual 错误 输入，例如使用 assert.strictEqual()。

 expected <any> 错误实例上的 expected 属性将包含此值。在内部用于 expected  错误输入，例如使用 assert.strictEqual()。

 operator <string> 错误实例上的 operator 属性将包含此值。在内部用于表明用于 比较的操作（或触发错误的断言函数）。

 stackStartFn <Function> 如果提供，则生成的堆栈跟踪将移除所有帧直到提供的函数。
```

```
new assert.AssertionError()
```

这种写法可以在不报错的情况下使用 `assert`.

但是实际情况不应该有这种，`assert` 毕竟会降低效率

只是应该在生产模式下使用，如果服务器遇到问题是否应该设置开关，来判断 `assert` 是否应该打开.

```
let n = 0;

var error = new assert.AssertionError({
    actual: n,
    operator: "deepStrictEqual",
    message: "deepStrictEqual assert 不通过"
});
```

可以通过 `operator` 模拟全部的 `assert` 操作. 并且在不中断程序运行的情况下返回值.

#### 2. strictEqual

比较浅层是否相等。不能比较对象，JSON等.

之前尝试了 `Equal` 应该和 `strictEqual` 差不多。

```
/** @deprecated since v9.9.0 - use strictEqual() instead. */
```

也就是说 `Equal`和 `strictEqual` 相同。

`strictEqual` =  `===`

#### 3. notStrictEqual

和 `strictEqual` 相反

#### 4. deepStrictEqual

比较是否相等。

> /** @deprecated since v9.9.0 - use deepStrictEqual() instead. */

同样，使用 `deepEqual` 也是调用 `deepStrictEqual`.

他会具体去遍历下层，原型，结果

```
const date = new Date();
const object = {};
const fakeDate = {};
Object.setPrototypeOf(fakeDate, Date.prototype);

// 原型不同：
assert.deepStrictEqual(object, fakeDate);

assert.deepStrictEqual(0, -0);

assert.deepStrictEqual(new Number(1), new Number(2));
```

#### 4. assert.throws

断言方法内肯定会出错.

```
assert.throws(fn, error? , message?)

assert.throws(() => { throw new TypeError("值错误") }, TypeError, "果然出错了");

```

#### 5. assert.doesNotThrow

和 `assert.throws` 相反，断言会出错

#### 6. assert.ifError

主要用途是在回调中测试 `error` 参数

```
assert.ifError(err);
```

#### 7. assert.rejects

和 `assert.throws` 基本一样，只不过是异步的.

#### 8. assert.doesNotReject

和 `assert.doesNotThrow` 一样，差别只是在异步上.

### javascript assert

在浏览器中的 `assert` 和 `nodejs assert` 最大的区别就是 ， 浏览器中不会中断程序进行，但是浏览器中的断言功能比较简单

```
console.assert(assertion, obj1 [, obj2, ..., objN]);
console.assert(assertion, msg [, subst1, ..., substN]); // c-like message formatting
```

```
const errorMsg = 'the # is not even';
for (let number = 2; number <= 5; number += 1) {
    console.log('the # is ' + number);
    console.assert(number % 2 === 0, {number: number, errorMsg: errorMsg});
    // or, using ES2015 object property shorthand:
    // console.assert(number % 2 === 0, {number, errorMsg});
}
```
超级简单，也太简单了，不支持 `function` `async` 等，请自行写好表达式。

### 结尾

我产生了一个疑问，在 `nodejs` 我是否还需要 `jest`. 普通项目我是否直接写断言就可以了？

只有在写前端 `javascript` 才需要 `jest` 去测试?

### 资料

[https://developer.mozilla.org/en-US/docs/Web/API/console/assert](https://developer.mozilla.org/en-US/docs/Web/API/console/assert)

[http://nodejs.cn/api/assert.html](http://nodejs.cn/api/assert.html)











