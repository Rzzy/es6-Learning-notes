## `for-of`
>for...of语句在可迭代对象（包括 Array，Map，Set，String，TypedArray，arguments 对象等等）上创建一个迭代循环，调用自定义迭代钩子，并为每个不同属性的值执行语句。

+ 推荐在循环对象属性的时候，使用for...in,在遍历数组的时候的时候使用for...of。
+ for...in循环出的是key，for...of循环出的是value。for...of修复了for...in的缺陷和不足，假设我们往数组添加一个属性name:
aArray.name = 'demo',再分别查看上面写的两个循环：

```javascript
for(let index in aArray){
    console.log(`${aArray[index]}`); //aArray.name会也被循环出来了，得到的数组的值的顺序也会被打乱
}
for(var value of aArray){
    console.log(value);
}
```
所以说，作用于数组的for-in循环除了遍历数组元素以外,还会遍历自定义属性。

for...of循环不会循环对象的key，只会循环出数组的value，因此for...of不能循环遍历普通对象,对普通对象的属性遍历推荐使用for...in

如果实在想用for...of来遍历普通对象的属性的话，可以通过和Object.keys()搭配使用，先获取对象的所有key的数组：
然后遍历：
```javascript
var student={
    name:'wujunchuan',
    age:22,
    locate:{
      country:'china',
      city:'xiamen',
      school:'XMUT'
    }
}
for(var key of Object.keys(student)){  //先获取studentkey的数组，循环数组再得到对象值，基本上是多此一举
    //使用Object.keys()方法获取对象key的数组
    console.log(key+": "+student[key]);
}
```

+ for...of不能循环普通的对象，需要通过和Object.keys()搭配使用

```javascript
// for...in循环可以遍历键名，for...of循环会报错。
var es6 = {
  edition: 6,
  committee: "TC39",
  standard: "ECMA-262"
};

for (e in es6) {
  console.log(e);
}
// edition
// committee
// standard

for (e of es6) {
  console.log(e);
}
// TypeError: es6 is not iterable
```
+ 与forEach()不同的是，它可以正确响应break、continue和return语句,forEach循环会简洁很多，但是不能break、continue、return

```javascript
var a = ["a", "b", "c"];
a.forEach(function(element) {
  console.log(element);
});
```
+ for...of现在浏览器的支持成都还不是很好
![](/assets/屏幕快照 2017-12-30 19.27.34.png)
![](/assets/屏幕快照 2017-12-30 19.27.19.png)


**语法**
```javascript
for (variable of iterable) {
    //statements
}
```
`variable`
在每次迭代中，将不同属性的值分配给变量。

`iterable`
可枚举其枚举属性的对象。

for...of循环可以使用的范围包括数组、类似数组的对象（比如arguments对象、DOM NodeList对象）、Set和Map结构、Generator对象，以及字符串
**示例**

迭代Array

```javascript
let iterable = [10, 20, 30];

for (let value of iterable) {
    value += 1;
    console.log(value);
}
// 11
// 21
// 31
```
如果你不想修改语句块中的变量 , 也可以使用const代替let。

```javascript
let iterable = [10, 20, 30];

for (const value of iterable) {
  console.log(value);
}
// 10
// 20
// 30

```

迭代String

```javascript
let iterable = "boo";

for (let value of iterable) {
  console.log(value);
}
// "b"
// "o"
// "o"
```

迭代 TypedArray

```javascript
let iterable = new Uint8Array([0x00, 0xff]);

for (let value of iterable) {
  console.log(value);
}
// 0
// 255
```

迭代Map

```javascript
let iterable = new Map([["a", 1], ["b", 2], ["c", 3]]);

for (let entry of iterable) {
  console.log(entry);
}
// ["a", 1]
// ["b", 2]
// ["c", 3]

for (let [key, value] of iterable) {
  console.log(value);
}
// 1
// 2
// 3
```

迭代 Set

```javascript
let iterable = new Set([1, 1, 2, 2, 3, 3]);

for (let value of iterable) {
  console.log(value);
}
// 1
// 2
// 3
```

迭代 arguments 对象

```javascript
(function() {
  for (let argument of arguments) {
    console.log(argument);
  }
})(1, 2, 3);

// 1
// 2
// 3
```

迭代 DOM 集合
迭代 DOM 元素集合，比如一个NodeList对象：下面的例子演示给每一个 article 标签内的 p 标签添加一个 "read" 类。

```javascript
//注意：这只能在实现了NodeList.prototype[Symbol.iterator]的平台上运行
let articleParagraphs = document.querySelectorAll("article > p");

for (let paragraph of articleParagraphs) {
  paragraph.classList.add("read");
}
```

关闭迭代器
对于for...of的循环，可以由break, continue[4], throw 或return[5]终止。在这些情况下，迭代器关闭。

```javascript
function* foo(){ 
  yield 1; 
  yield 2; 
  yield 3; 
}; 

for (let o of foo()) { 
  console.log(o); 
  break; // closes iterator, triggers return
}
```


迭代生成器
你还可以迭代一个生成器：

```javascript
function* fibonacci() { // 一个生成器函数
    let [prev, curr] = [0, 1];
    for (;;) { // while (true) {
        [prev, curr] = [curr, prev + curr];
        yield curr;
    }
}
 
for (let n of fibonacci()) {
     console.log(n); 
    // 当n大于1000时跳出循环
    if (n >= 1000)
        break;
}
```

不要重用生成器
生成器不应该重用，即使for...of循环的提前终止，例如通过break关键字。在退出循环后，生成器关闭，并尝试再次迭代，不会产生任何进一步的结果。

```javascript
var gen = (function *(){
    yield 1;
    yield 2;
    yield 3;
})();
for (let o of gen) {
    console.log(o);
    break;//关闭生成器
} 

//生成器不应该重用，以下没有意义！
for (let o of gen) {
    console.log(o);
}
```

迭代其他可迭代对象
你还可以迭代显式实现可迭代协议的对象：

```javascript

var iterable = {
  [Symbol.iterator]() {
    return {
      i: 0,
      next() {
        if (this.i < 3) {
          return { value: this.i++, done: false };
        }
        return { value: undefined, done: true };
      }
    };
  }
};

for (var value of iterable) {
  console.log(value);
}
// 0
// 1
// 2
```

for...of与for...in的区别
无论是for...in还是for...of语句都是迭代一些东西。它们之间的主要区别在于它们的迭代方式。

for...in 语句以原始插入顺序迭代对象的可枚举属性。

for...of 语句遍历可迭代对象定义要迭代的数据。

以下示例显示了与Array一起使用时，for...of循环和for...in循环之间的区别。

```javascript
Object.prototype.objCustom = function() {}; 
Array.prototype.arrCustom = function() {};

let iterable = [3, 5, 7];
iterable.foo = 'hello';

for (let i in iterable) {
  console.log(i); // logs 0, 1, 2, "foo", "arrCustom", "objCustom"
}

for (let i in iterable) {
  if (iterable.hasOwnProperty(i)) {
    console.log(i); // logs 0, 1, 2, "foo"
  }
}

for (let i of iterable) {
  console.log(i); // logs 3, 5, 7
}
Object.prototype.objCustom = function() {};
Array.prototype.arrCustom = function() {}; 

let iterable = [3, 5, 7]; 
iterable.foo = 'hello';
```

每个对象将继承objCustom属性，并且作为Array的每个对象将继承arrCustom属性，因为将这些属性添加到Object.prototype和Array.prototype。由于继承和原型链，对象iterable继承属性objCustom和arrCustom。

```javascript
for (let i in iterable) {
  console.log(i); // logs 0, 1, 2, "foo", "arrCustom", "objCustom" 
}
```

此循环仅以原始插入顺序记录iterable 对象的可枚举属性。它不记录数组元素3, 5, 7 或hello，因为这些不是枚举属性。但是它记录了数组索引以及arrCustom和objCustom。如果你不知道为什么这些属性被迭代，array iteration and for...in中有更多解释。

```javascript
for (let i in iterable) {
  if (iterable.hasOwnProperty(i)) {
    console.log(i); // logs 0, 1, 2, "foo"
  }
}
```
这个循环类似于第一个，但是它使用hasOwnProperty() 来检查，如果找到的枚举属性是对象自己的（不是继承的）。如果是，该属性被记录。记录的属性是0, 1, 2和foo，因为它们是自身的属性（不是继承的）。属性arrCustom和objCustom不会被记录，因为它们是继承的。

```javascript
for (let i of iterable) {
  console.log(i); // logs 3, 5, 7 
}
```
该循环迭代并记录iterable作为可迭代对象定义的迭代值，这些是数组元素 3, 5, 7，而不是任何对象的属性。








