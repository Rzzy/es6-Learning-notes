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
-不符合标准的迭代器-


