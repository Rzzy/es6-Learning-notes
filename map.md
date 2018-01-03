### Map
>Map 对象保存键值对。任何值(对象或者原始值) 都可以作为一个键或一个值。

##### 语法

```javaScript
new Map([iterable])
```

##### 参数

`iterable`:`Iterable` 可以是一个数组或者其他 `iterable` 对象，其元素或为键值对，或为两个元素的数组。 每个键值对都会添加到新的 `Map`。`null` 会被当做 `undefined`。

##### 描述
一个`Map`对象以插入顺序迭代其元素 — 一个 ` for...of` 循环为每次迭代返回一个`[key，value]`数组。

###### 键的相等(Key equality)
键的比较是基于 `"SameValueZero"` 算法：`NaN` 是与 `NaN` 相同的（虽然 `NaN !== NaN`），剩下所有其它的值是根据 `===` 运算符的结果判断是否相等。在目前的`ECMAScript`规范中，`-0`和`+0`被认为是相等的，尽管这在早期的草案中并不是这样。有关详细信息，请参阅浏览器兼容性 表中的`“value equality for -0 and 0”`。

###### `Objects` 和 `maps` 的比较
`Object` 和 `Map`类似的一点是,它们都允许你按键存取一个值,都可以删除键,还可以检测一个键是否绑定了值.因此,一直以来,我们都把对象当成Map来使用,不过,现在有了`Map`,下面的区别解释了为什么使用`Map`更好点.
+ 一个对象通常都有自己的原型,所以一个对象总有一个"prototype"键。不过，从 ES5 开始可以使用 map = Object.create(null)来创建一个没有原型的对象。
+ 一个对象的键只能是字符串或者 `Symbols`，但一个 `Map` 的键可以是任意值。
+ 你可以通过`size`属性很容易地得到一个`Map`的键值对个数，而对象的键值对个数只能手动确认。

但是这并不意味着你可以随意使用 `Map`，对象仍旧是最常用的。`Map` 实例只适合用于集合(`collections`)，你应当考虑修改你原来的代码——先前使用对象来处理集合的地方。对象应该用其字段和方法来作为记录的。

如果你不确定要使用哪个，请思考下面的问题：
+ 在运行之前 key 是否是未知的，是否需要动态地查询 key 呢？
+ 是否所有的值都是统一类型，这些值可以互换么？
+ 是否需要不是字符串类型的 key ？
+ 键值对经常增加或者删除么？
+ 是否有任意个且非常容易改变的键值对?
+ 这个集合可以遍历么(Is the collection iterated)?

假如以上全是“是”的话，那么你需要用` Map` 来保存这个集。 相反，你有固定数目的键值对，独立操作它们，区分它们的用法，那么你需要的是对象。

##### 属性

|属性||
|-|-|
|`Map.length`|属性 length 的值为 0 。|
|`get Map[@@species]`|本构造函数用于创建派生对象。|
|`Map.prototype`|表示 `Map `构造器的原型。 允许添加属性从而应用于所有的` Map` 对象。|

##### `Map` 实例

所有的 `Map` 对象实例都会继承 `Map.prototype`。

|属性||
|-|-|
|`Map.prototype.constructor`|返回一个函数，它创建了实例的原型。默认是`Map`函数。|
|`Map.prototype.size`|返回`Map`对象的键/值对的数量。|

|方法||
|-|-|
|`Map.prototype.clear()`|移除Map对象的所有键/值对 。|
|`Map.prototype.delete(key)`|移除任何与键相关联的值，并且返回该值，该值在之前会被`Map.prototype.has(key)`返回为`true`。之后再调用`Map.prototype.has(key)`会返回`false`。|
|`Map.prototype.entries()`|返回一个新的 `Iterator` 对象，它按插入顺序包含了`Map`对象中每个元素的 `[key, value]` 数组。|
|`Map.prototype.forEach(callbackFn[, thisArg])`|按插入顺序，为 `Map`对象里的每一键值对调用一次`callbackFn`函数。如果为`forEach`提供了`thisArg`，它将在每次回调中作为`this`值。|
|`Map.prototype.get(key)`|返回键对应的值，如果不存在，则返回`undefined`。|
|`Map.prototype.has(key)`|返回一个布尔值，表示`Map`实例是否包含键对应的值。|
|`Map.prototype.keys()`|返回一个新的 `Iterator`对象， 它按插入顺序包含了Map对象中每个元素的键 。|
|`Map.prototype.set(key, value)`|设置Map对象中键的值。返回该Map对象。|
|`Map.prototype.values()`|返回一个新的`Iterator`对象，它按插入顺序包含了`Map`对象中每个元素的值 。|
|`Map.prototype[@@iterator]()`|返回一个新的`Iterator`对象，它按插入顺序包含了`Map`对象中每个元素的 `[key, value] `数组。|


##### 示例
使用映射对象

```javaScript
var myMap = new Map();
 
var keyObj = {},
    keyFunc = function () {},
    keyString = "a string";
 
// 添加键
myMap.set(keyString, "和键'a string'关联的值");
myMap.set(keyObj, "和键keyObj关联的值");
myMap.set(keyFunc, "和键keyFunc关联的值");
 
myMap.size; // 3
 
// 读取值
myMap.get(keyString);    // "和键'a string'关联的值"
myMap.get(keyObj);       // "和键keyObj关联的值"
myMap.get(keyFunc);      // "和键keyFunc关联的值"
 
myMap.get("a string");   // "和键'a string'关联的值"
                         // 因为keyString === 'a string'
myMap.get({});           // undefined, 因为keyObj !== {}
myMap.get(function() {}) // undefined, 因为keyFunc !== function () {}
```

将`NaN`作为映射的键
`NaN `也可以作为`Map`对象的键. 虽然 `NaN` 和任何值甚至和自己都不相等`(NaN !== NaN 返回true)`, 但下面的例子表明, 两个`NaN`作为`Map`的键来说是没有区别的:

```javaScript
var myMap = new Map();
myMap.set(NaN, "not a number");

myMap.get(NaN); // "not a number"

var otherNaN = Number("foo");
myMap.get(otherNaN); // "not a number"
```

使用`for..of`方法迭代映射
映射也可以使用`for..of`循环来实现迭代：

```javaScript
var myMap = new Map();
myMap.set(0, "zero");
myMap.set(1, "one");
for (var [key, value] of myMap) {
  console.log(key + " = " + value);
}
// 将会显示两个log。一个是"0 = zero"另一个是"1 = one"

for (var key of myMap.keys()) {
  console.log(key);
}
// 将会显示两个log。 一个是 "0" 另一个是 "1"

for (var value of myMap.values()) {
  console.log(value);
}
// 将会显示两个log。 一个是 "zero" 另一个是 "one"

for (var [key, value] of myMap.entries()) {
  console.log(key + " = " + value);
}
// 将会显示两个log。 一个是 "0 = zero" 另一个是 "1 = one"
```

使用forEach()方法迭代映射
映射也可以通过forEach()方法迭代：

```javaScript
myMap.forEach(function(value, key) {
  console.log(key + " = " + value);
}, myMap)
// 将会显示两个logs。 一个是 "0 = zero" 另一个是 "1 = one"
```

映射与数组对象的关系

```javaScript
var kvArray = [["key1", "value1"], ["key2", "value2"]];

// 使用映射对象常规的构造函数将一个二维键值对数组对象转换成一个映射关系
var myMap = new Map(kvArray);

myMap.get("key1"); // 返回值为 "value1"

// 使用展开运算符将一个映射关系转换成一个二维键值对数组对象
console.log(uneval([...myMap])); // 将会向您显示和kvArray相同的数组

// 或者使用展开运算符作用在键或者值的迭代器上，进而得到只含有键或者值得数组
console.log(uneval([...myMap.keys()])); // 输出 ["key1", "key2"]
```







