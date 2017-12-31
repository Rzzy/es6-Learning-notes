### iterator 迭代器
迭代器（Iterator）是一个接口，为各种不同的数据结构提供统一的访问机制。任何数据只要部署了 Iterator 接口，就可以完成遍历操作。

**Symbol.iterator 属性的属性特性：**

属性|属性特性
----|---
`writable:` | `false`
`enumerable:` | `false`
`configurable:` | `false`

**描述**
>当需要对一个对象进行迭代时（比如开始用于一个for..of循环中），它的@@iterator方法都会在不传参情况下被调用，返回的迭代器用于获取要迭代的值。

一些内置类型拥有默认的迭代器行为，其他类型（如 Object）则没有。下表中的内置类型拥有默认的@@iterator方法：
+ Array.prototype[@@iterator]()
+ TypedArray.prototype[@@iterator]()
+ String.prototype[@@iterator]()
+ Map.prototype[@@iterator]()
+ Set.prototype[@@iterator]()

**作用**
+ 各种数据结构提供一个统一的、简便的访问接口；
+ 使数据结构成员能够按某种次序排列；
+ ES6 创造了一种新的遍历命令 for...of 循环，Iterator 接口主要供 for…of 消费

就拿 for…of 语句来说，它首先调用被遍历对象的 [Symbol.iterator]() 方法，该方法返回一个迭代器对象，迭代器对象可以是拥有 next 方法的任何对象。然后， 在 for…of 的每一次循环中，都将调用该迭代器对象上的 next 方法。
每一次调用 next 方法，都会返回数据结构的当前成员信息。具体来说就是返回一个包含 value 和 done 两个属性的对象。其中 value 是当前成员的值，done 是一 个布尔值，表示遍历是否结束。

下面的代码实现了一个简单的迭代器对象：

```javascript
var sampleIterator = {
  index: 0,
  [Symbol.iterator]: function() {
    return this
  },
  next: function() {
    if (this.index < 3) {
      return {
        done: false,
        value: this.index++
      }
    } else {
      return {
        done: true,
        value: undefined
      }
    }
  }
}
for (var val of sampleIterator) {
  console.log(val)
}
// 结果为：
// 0
// 1
// 2
// [Finished in 2.7s]
```  
上面的代码中，当使用 `for…of` 遍历 `sampleIterator` 时，首先调用了该对象的 `[Symool.itirator]` 方法，该方法返回对象本身。而该对象中包含有 `next` 方法，所以该对象本身就是一个 `Iterator `对象。可以供 `for..of `消费。当 `this.index >= 3` 时，返回 `{done: true, value: undefined}`, 循环结束。




**示例**

*自定义迭代器*
我们可以像下面这样创建自定义的迭代器：

```javascript
var myIterable = {}
myIterable[Symbol.iterator] = function* () {
    yield 1;
    yield 2;
    yield 3;
};
[...myIterable] // [1, 2, 3]
```
*不符合标准的迭代器*
如果一个迭代器 @@iterator 没有返回一个迭代器对象，那么它就是一个不符合标准的迭代器，这样的迭代器将会在运行期抛出异常，甚至非常诡异的 Bug。

```javascript
var nonWellFormedIterable = {}
nonWellFormedIterable[Symbol.iterator] = () => 1
[...nonWellFormedIterable] // TypeError: [] is not a function
```



