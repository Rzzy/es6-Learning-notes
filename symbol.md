### 深入解析 ES6：`Symbol`

>引用：[http://bubkoo.com/2015/07/24/es6-in-depth-symbols/](http://bubkoo.com/2015/07/24/es6-in-depth-symbols/)

**`Symbol` 是什么？**

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

**获取 `Symbol` 的三种方法**
+ `Symbol()` 每次调用时都返回一个唯一的 Symbol。
+ `Symbol.for(string)` 从 `Symbol `注册表中返回相应的 `Symbol`，与上个方法不同的是，`Symbol `注册表中的 `Symbol` 是共享的。也就是说，如果你调用 `Symbol.for("cat")` 三次，都将返回相同的 `Symbol`。当不同页面或同一页面不同模块需要共享 `Symbol` 时，注册表就非常有用。
+ `Symbol.iterator` 返回语言预定义的一些 `Symbol`，每个都有其特殊的用途。

如果你仍不确定 `Symbol` 是否有用，那么接下来的内容将非常有趣，因为我将为你演示 `Symbol` 的实际应用。

**`Symbol` 在 `ES6` 规范中的应用**
+ 可以使用 `Symbol` 来避免代码冲突。
+ 在使用 `iterator` 时，我们解析`for (var item of myArray)` 内部是以调用 `myArray[Symbol.iterator]()` 开始的，这个方法可以使用 `myArray.iterator()` 来代替，但是使用 `Symbol` 的后向兼容性更好。

在 `ES6` 中还有一些地方使用到了 `Symbol`。（这些特性还没有在 `FireFox` 中实现。）

+ 使 `instanceof` 可扩展。在 `ES6 `中，`object instanceof constructor` 表达式被标准化为构造函数的一个方法：`constructor[Symbol.hasInstance](object)`，这意味着它是可扩展的。
+ 消除新特性和旧代码之间的冲突。
+ 支持新类型的字符串匹配。在 `ES5` 中，调用 `str.match(myObject)` 时，首先会尝试将 `myObject `转换为 `RegExp `对象。在 `ES6 `中，首先将检查 `myObject `中是否有 `myObject[Symbol.match](str)` 方法，在所有正则表达式工作的地方都可以提供一个自定义的字符串解析方法。

这些用途还比较窄，但仅仅通过我文章中的代码很难看到这些新特性产生的重大影响。JavaScript 的 Symbol 是 PHP 和 Python 中 __doubleUnderscores 的改进版本，标准组织将使用它来为语言添加新特性，而不会对已有代码产生影响。

**案例**
*一个布尔值引出的问题：*
有时，把一些属于其他对象的数据暂存在另一个对象中是非常方便的。例如，假设你正在编写一个 `JS` 库，使用 `CSS` 中的 `transition` 来让一个 `DOM `元素在屏幕上飞奔，你已经知道不能同时将多个 `transition` 应用在同一个 `div `上，否则将使得动画非常不美观，你也确实有办法来解决这个问题，但是首先你需要知道该 `div `是否已经在移动中。

怎么解决这个问题呢？

其中一个方法是使用浏览器提供的 API 来探测元素是否处于动画状态，但杀鸡焉用牛刀，在将元素设置为移动时，你的库就知道了该元素正在移动。

你真正需要的是一种机制来跟踪哪些元素正在移动，你可以将正在移动的元素保存在一个数组中，每次要为一个元素设置动画时，首先检查一下这个元素是否已经在这个列表中。

啊哈，但是如果你的数组非常庞大，即便是这样的线性搜索也会产生性能问题。

那么，你真正想做的就是直接在元素上设置一个标志：

```javaScript

if (element.isMoving) {
  smoothAnimations(element);
}
element.isMoving = true;

```

**兼容性**
对于还没有原生支持 Symbol 的浏览器，你可以使用 polyfill，如 [core.js](https://github.com/zloirock/core-js#ecmascript-6-symbols)，但该 polyfill 实现并不完美，请阅读[注意事项](https://github.com/zloirock/core-js#caveats-when-using-symbol-polyfill)。














