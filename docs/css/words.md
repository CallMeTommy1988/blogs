# words 文字处理

### 1. word-wrap

```
word-wrap: normal|break-word;
```

> normal	只在允许的断字点换行（浏览器保持默认处理）。

> break-word	在长单词或 URL 地址内部进行换行。

其实很好理解。就是自动换行。

有时候会遇到文本过长的时候，英文文本过长且没有标点符号，空格之类的时候，是不会换行的。 会直接撑破容器。

这个时候

```
word-wrap: break-word;
```

会让英文自动换行，他的特点是不断单词。

也就是说，如果下一个单词超过了所剩下的距离，下一个单词会重新在一行开始.

这是他的特点也是他的问题

举个例子

```
<div class="demo1">
aaaaaaaaaaaaaaa aaaaaaaaaaaaaaa aaaaaaaaaaaaaaa
</div>
```
我们假设里面的单词 `aaaa...` 正好都比宽度刚好多一个字母。

类似以下

aaaaaaaaaaaaaa
a
aaaaaaaaaaaaaa
a
aaaaaaaaaaaaaa
a

那么得到的结果就是，奇数行都是满的，偶数行正好都只一个有个。

所以这里的断行还是要根据情况合理运用。

### 2. word-break

基于上面的断行方式,于是有新的方式

- normal	使用浏览器默认的换行规则。
- break-all	允许在单词内换行。
- keep-all	只能在半角空格或连字符处换行。

`normal` 和 `keep-all` 一样.

也就是如果是 纯英文且不断行的话,就和普通浏览器方式一样. **超出**

如果使用 `break-all` 的话


aaaaaaaaaaaaaa
a   aaaaaaaaaaaaa
aa  aaaaaaaaaaaa
aaa

也就是说会在单词内断行, 他比

```
word-wrap: break-word;
```

更进一步, 他能直接断行. 

但是问题就是, 会直接断开单词. 造成了用户的阅读困难.
 
### 3.white-space

浏览器空白如何处理

- normal	默认。空白会被浏览器忽略。
- pre	空白会被浏览器保留。其行为方式类似 HTML 中的` <pre>` 标签。
- nowrap	文本不会换行，文本会在在同一行上继续，直到遇到 `<br>` 标签为止。
- pre-wrap	保留空白符序列，但是正常地进行换行。
- pre-line	合并空白符序列，但是保留换行符。
- inherit	规定应该从父元素继承 `white-space` 属性的值。

就是字面上的意思


### 以上




