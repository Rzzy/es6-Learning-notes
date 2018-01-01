### Set & Map

> Set 对象允许你存储任何类型的唯一值，无论是原始值或者是对象引用。

**语法**

```javaScript
new Set([iterable]);
```
**参数**
`iterable`
如果传递一个可迭代对象，它的所有元素将被添加到新的 `Set`中。如果不指定此参数或其值为`null`，则新的`Set`为空。

**返回值**
一个新的`Set`对象。

**简述**
`Set`对象是值的集合，你可以按照插入的顺序迭代它的元素。 `Set`中的元素只会出现一次，即 `Set` 中的元素是唯一的。

**值的相等**
因为 `Set` 中的值总是唯一的，所以需要判断两个值是否相等。在ECMAScript规范的早期版本中，这不是基于和===操作符中使用的算法相同的算法。具体来说，对于`Set s`， +0 （+0 严格相等于-0）和-0是不同的值。然而，在 ECMAScript 2015规范中这点已被更改。有关详细信息，请参阅浏览器兼容性 表中的“value equality for -0 and 0”。

另外，`NaN`和`undefined`都可以被存储在`Set` 中， `NaN`之间被视为相同的值（尽管 `NaN !== NaN`）。


**属性**
`Set.length`:
`length`属性的值为0。

`get Set[@@species]`:
构造函数用来创建派生对象.

`Set.prototype`:
表示`Set`构造器的原型，允许向所有`Set`对象添加新的属性。

**`Set`实例**
所有`Set`实例继承自 `Set.prototype`。

***属性***

|ddd|ssss|
---|---|
`Set.prototype.constructor`|返回实例的构造函数。默认情况下是Set。



`Set.prototype.constructor`
返回实例的构造函数。默认情况下是Set。

`Set.prototype.size`
返回Set对象的值的个数。

***方法***






























