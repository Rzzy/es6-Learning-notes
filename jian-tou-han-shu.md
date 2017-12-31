### 箭头函数
>箭头函数表达式的语法比函数表达式更短，并且不绑定自己的this，arguments，super或 new.target。这些函数表达式最适合用于非方法函数，并且它们不能用作构造函数。

**语法**

```javascript
基础语法

(参数1, 参数2, …, 参数N) => {函数声明}
(参数1, 参数2, …, 参数N) => 表达式（单一）
//相当于：(参数1, 参数2, …, 参数N) =>{ return表达式}

// 当只有一个参数时，圆括号是可选的：
(单一参数) => {函数声明}
单一参数 => {函数声明}

// 没有参数的函数应该写成一对圆括号。
() => {函数声明}
```
**高级语法**

```javascript
//加括号的函数体返回对象字面表达式：
参数=> ({foo: bar})

//支持剩余参数和默认参数
(参数1, 参数2, ...rest) => {函数声明}
(参数1 = 默认值1,参数2, …, 参数N = 默认值N) => {函数声明}

//同样支持参数列表解构
let f = ([a, b] = [1, 2], {x: c} = {x: a + b}) => a + b + c;
f();  // 6

```

**描述**
>引入箭头函数有两个方面的作用：更简短的函数并且不绑定this。

*更短的函数*

```javascript
var materials = [
  'Hydrogen',
  'Helium',
  'Lithium',
  'Beryllium'
];
materials.map(function(material) { 
  return material.length; 
}); // [8, 6, 7, 9]

materials.map((material) => {
  return material.length;
}); // [8, 6, 7, 9]

materials.map(material => material.length); // [8, 6, 7, 9]
```
*不绑定`this`*
>在箭头函数出现之前，每个新定义的函数都有它自己的 this值（在构造函数的情况下是一个新对象，在严格模式的函数调用中为 undefined，如果该函数被称为“对象方法”则为基础对象等）。This被证明是令人厌烦的面向对象风格的编程。

```javascript
function Person() {
  // Person() 构造函数定义 `this`作为它自己的实例.
  this.age = 0;

  setInterval(function growUp() {
    // 在非严格模式, growUp()函数定义 `this`作为全局对象, 
    // 与在 Person()构造函数中定义的 `this`并不相同.
    this.age++;
  }, 1000);
}

var p = new Person();
```
在ECMAScript 3/5中，通过将this值分配给封闭的变量，可以解决this问题。

```javascript
function Person() {
  var that = this;
  that.age = 0;

  setInterval(function growUp() {
    //  回调引用的是`that`变量, 其值是预期的对象. 
    that.age++;
  }, 1000);
}

```
或者，可以创建绑定函数，以便将预先分配的`this`值传递到绑定的目标函数（上述示例中的`growUp()`函数）。

箭头功能不会创建自己的`this`；它使用封闭执行上下文的`this`值。因此，在下面的代码中，传递给`setInterval`的函数内的`this`与封闭函数中的`this`值相同：

```javascript
function Person(){
  this.age = 0;

  setInterval(() => {
    this.age++; // |this| 正确地指向person 对象
  }, 1000);
}

var p = new Person();
```





























