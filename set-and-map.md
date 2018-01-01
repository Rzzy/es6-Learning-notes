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


|属性|introduce|
|---|---|
|`Set.prototype.constructor`|返回实例的构造函数。默认情况下是Set|
|`Set.prototype.size`|返回Set对象的值的个数|

***方法***

|方法|introduce|
|---|---|
|`Set.prototype.add(value)`|在Set对象尾部添加一个元素。返回该Set对象。|
|`Set.prototype.clear()`|移除Set对象内的所有元素。|
|`Set.prototype.delete(value)`|移除`Set`的中与这个值相等的元素，返回`Set.prototype.has(value)`在这个操作前会返回的值（即如果该元素存在，返回`true`，否则返回`false`）。`Set.prototype.has(value)`在此后会返回`false`。|
|`Set.prototype.entries()`|返回一个新的迭代器对象，该对象包含Set对象中的按插入顺序排列的所有元素的值的[value, value]数组。为了使这个方法和Map对象保持相似， 每个值的键和值相等。|
|`Set.prototype.forEach(callbackFn[, thisArg])`|按照插入顺序，为`Set对`象中的每一个值调用一次`callBackFn`。如果提供了`thisArg`参数，回调中的`this`会是这个参数。|
|`Set.prototype.has(value)`|返回一个布尔值，表示该值在`Set`中存在与否。|
|`Set.prototype.keys()`|与`values()`方法相同，返回一个新的迭代器对象，该对象包含`Set`对象中的按插入顺序排列的所有元素的值。|
|`Set.prototype.values()`|返回一个新的迭代器对象，该对象包含Set对象中的按插入顺序排列的所有元素的值。|
|`Set.prototype[@@iterator]()`|返回一个新的迭代器对象，该对象包含`Set`对象中的按插入顺序排列的所有元素的值。|


**示例**
使用Set对象

```javaScript
let mySet = new Set();

mySet.add(1); // Set(1) {1}
mySet.add(5); // Set(2) {1, 5}
mySet.add(5); // Set { 1, 5 }
mySet.add("some text"); // Set(3) {1, 5, "some text"}
var o = {a: 1, b: 2};
mySet.add(o);

mySet.add({a: 1, b: 2}); // o 指向的是不同的对象，所以没问题

mySet.has(1); // true
mySet.has(3); // false
mySet.has(5);              // true
mySet.has(Math.sqrt(25));  // true
mySet.has("Some Text".toLowerCase()); // true
mySet.has(o); // true

mySet.size; // 5

mySet.delete(5);  // true,  从set中移除5
mySet.has(5);     // false, 5已经被移除

mySet.size; // 4, 刚刚移除一个值
console.log(mySet); // Set {1, "some text", Object {a: 1, b: 2}, Object {a: 1, b: 2}}
```

Set不支持索引

```javaScript
arr.indexOf('a') !== -1 //慢
//true
setOfWords.has('a') //快 
//true
```

迭代Set

```javaScript
// 迭代整个set
// 按顺序输出：1, "some text" 
for (let item of mySet) console.log(item);

// 按顺序输出：1, "some text" 
for (let item of mySet.keys()) console.log(item);
 
// 按顺序输出：1, "some text" 
for (let item of mySet.values()) console.log(item);

// 按顺序输出：1, "some text" 
//(键与值相等)
for (let [key, value] of mySet.entries()) console.log(key);

// 转换Set为Array (with Array comprehensions)
var myArr = [v for (v of mySet)]; // [1, "some text"]
// 替代方案(with Array.from)
var myArr = Array.from(mySet); // [1, "some text"]

// 如果在HTML文档中工作，也可以：
mySet.add(document.body);
mySet.has(document.querySelector("body")); // true

// Set 和 Array互换
mySet2 = new Set([1,2,3,4]);
mySet2.size; // 4
[...mySet2]; // [1,2,3,4]

// intersect can be simulated via 
var intersection = new Set([...set1].filter(x => set2.has(x))); // 新的迭代器对象

// difference can be simulated via
var difference = new Set([...set1].filter(x => !set2.has(x)));

// 用forEach迭代
mySet.forEach(function(value) {
  console.log(value);
});

// 1
// 2
// 3
// 4

```

`Array` 相关

```javaScript
var myArray = ["value1", "value2", "value3"];

// 用Set构造器将Array转换为Set
var mySet = new Set(myArray);

mySet.has("value1"); // returns true

// 用...(展开操作符)操作符将Set转换为Array
console.log([...mySet]); // 与myArray完全一致
```

`String` 相关

```javaScript
var text = 'Indiana';

var mySet = new Set(text);  // Set {'I', 'n', 'd', 'i', 'a'}
mySet.size;  // 5

```
![](/assets/屏幕快照 2018-01-02 00.35.42.png)
![](/assets/屏幕快照 2018-01-02 00.35.23.png)
























































































































































































































































































































































































































































































































































































































































































