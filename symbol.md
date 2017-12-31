### 深入解析 ES6：`Symbol`

>引用：[http://bubkoo.com/2015/07/24/es6-in-depth-symbols/](http://bubkoo.com/2015/07/24/es6-in-depth-symbols/)

**Symbol 是什么？**

```javaScript
typeof Symbol()
"symbol"
```
+ `JavaScript` 在 1997 年被标准化时，就有 6 种数据类型，直到 ES6 出现之前，程序中的变量一定是以下 6 种数据类型之一：
    + Undefined
    + Null
    + Boolean
    + Number
    + String
    + Object
    而symbol则是javascript中的第**七**种数据类型

+ `Symbol` 是完全不一样的东西。一旦创建后就不可更改，不能对它们设置属性（如果在严格模式下尝试这样做，你将得到一个 `TypeError`）。它们可以作为属性名，这时它们和字符串的属性名没有什么区别。

+ 每个 `Symbol` 都是独一无二的，不与其它 `Symbol` 重复（即便是使用相同的 `Symbol` 描述创建），创建一个 `Symbol` 就跟创建一个对象一样方便。

+ `ES6` 中的 `Symbol `与传统语言（如 `Lisp` 和 `Ruby`）中的 `Symbol` 中的类似，但并不是完全照搬到 `JavaScript` 中。在 `Lisp` 中，所有标识符都是 `Symbol`；在 `JavaScript` 中，标识符和大多数属性仍然是字符串，`Symbol` 只是提供了一个额外的选择。

+ 值得注意的是：与其它类型不同的是，`Symbol` 不能自动被转换为字符串，当尝试将一个 `Symbol` 强制转换为字符串时，将返回一个 `TypeError`。

```javaScript
> var sym = Symbol("<3");
> "your symbol is " + sym
// TypeError: can't convert symbol to string
> `your symbol is ${sym}`
// TypeError: can't convert symbol to string

```
应该避免这样的强制转换，应该使用 `String(sym)` 或 `sym.toString()` 来转换。