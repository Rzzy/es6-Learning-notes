### 箭头函数

> 箭头函数表达式的语法比函数表达式更短，并且不绑定自己的this，arguments，super或 new.target。这些函数表达式最适合用于非方法函数，并且它们不能用作构造函数。

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

> 引入箭头函数有两个方面的作用：更简短的函数并且不绑定this。

_更短的函数_

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

_不绑定_`this`

> 在箭头函数出现之前，每个新定义的函数都有它自己的 this值（在构造函数的情况下是一个新对象，在严格模式的函数调用中为 undefined，如果该函数被称为“对象方法”则为基础对象等）。This被证明是令人厌烦的面向对象风格的编程。

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

**与严格模式的关系**  
鉴于 this 是词法层面上的，严格模式中与 this 相关的规则都将被忽略。

```javascript
var f = () => {'use strict'; return this};
f() === window; // 或全局对象
```

严格模式的其他规则依然不变.

**通过 call 或 apply 调用**  
由于 this 已经在词法层面完成了绑定，通过 call\(\) 或 apply\(\) 方法调用一个函数时，只是传入了参数而已，对 this 并没有什么影响：

```javascript
var adder = {
  base : 1,

  add : function(a) {
    var f = v => v + this.base;
    return f(a);
  },

  addThruCall: function(a) {
    var f = v => v + this.base;
    var b = {
      base : 2
    };

    return f.call(b, a);
  }
};

console.log(adder.add(1));         // 输出 2
console.log(adder.addThruCall(1)); // 仍然输出 2（而不是3 ——译者注）
```

**不绑定**`arguments`  
箭头函数不绑定Arguments 对象。因此，在本示例中，参数只是在封闭范围内引用相同的名称：

```javascript
var arguments = 42;
var arr = () => arguments;

arr(); // 42

function foo() {
  var f = (i) => arguments[0]+i;  // 此处的argument为foo的arguments
  // foo函数的间接参数绑定
  return f(2);
}

foo(1); // 3
```

在大多数情况下，使用剩余参数是使用arguments对象的好选择。

```javascript
function foo() { 
  var f = (...args) => args[0]; 
  return f(2); 
}

foo(1); 
// 2
```

**像方法一样使用箭头函数**  
如上所述，箭头函数表达式对非方法函数是最合适的。让我们看看当我们试着把它们作为方法时发生了什么。

```javascript
'use strict';
var obj = {
  i: 10,
  b: () => console.log(this.i, this),
  c: function() {
    console.log( this.i, this)
  }
}
obj.b(); 
// undefined
obj.c(); 
// 10, Object {...}
```

箭头函数没有定义this绑定。另一个涉及Object.defineProperty\(\)的示例：

```javascript
'use strict';
var obj = {
  a: 10
};

Object.defineProperty(obj, "b", {
  get: () => {
    console.log(this.a, typeof this.a, this);
    return this.a+10; 
   // 代表全局对象 'Window', 因此 'this.a' 返回 'undefined'
  }
});
```

**使用 new 操作符**  
箭头函数不能用作构造器，和 new一起用会抛出错误。

```javascript
var Foo = () => {};
var foo = new Foo(); // TypeError: Foo is not a constructor
```

**使用prototype属性**  
箭头函数没有prototype属性。

```javascript
var Foo = () => {};
console.log(Foo.prototype); // undefined
```

**使用 yield 关键字**

> yield 关键字通常不能在箭头函数中使用（除非是嵌套在允许使用的函数内）。因此，箭头函数不能用作生成器。

**函数体**  
箭头功能可以有一个“简写体”或常见的“块体”。

> 在一个简写体中，只需要一个表达式，并附加一个隐式的返回值。在块体中，必须使用明确的return语句。

```javascript
var func = x => x * x;                  
// 简写函数 省略return

var func = (x, y) => { return x + y; }; 
//常规编写 明确的返回值
```

**返回对象字面量**  
记住用params =&gt; {object:literal}这种简单的语法返回对象字面量是行不通的。

```javascript
var func = () => { foo: 1 };               
// Calling func() returns undefined!

var func = () => { foo: function() {} };   
// SyntaxError: function statement requires a name
```

这是因为花括号（{} ）里面的代码被解析为一系列语句（即 foo 被认为是一个标签，而非对象字面量的组成部分）。

所以，记得用圆括号把对象字面量包起来：

```javascript
var func = () => ({foo: 1});
```

**换行**  
箭头函数在参数和箭头之间不能换行。

```javascript
var func = ()
           => 1; 
// SyntaxError: expected expression, got '=>'
```

**解析顺序**  
虽然箭头函数中的箭头不是运算符，但箭头函数具有与常规函数不同的特殊运算符优先级解析规则。

```javascript
let callback;

callback = callback || function() {}; // ok

callback = callback || () => {};      
// SyntaxError: invalid arrow-function arguments

callback = callback || (() => {});    // ok
```

**更多示例**

```javascript
// 空的箭头函数返回 undefined
let empty = () => {};

(() => 'foobar')(); 
// Returns "foobar"
// (这是一个立即执行函数表达式,可参阅 'IIFE'术语表) 


var simple = a => a > 15 ? 15 : a; 
simple(16); // 15
simple(10); // 10

let max = (a, b) => a > b ? a : b;

// Easy array filtering, mapping, ...

var arr = [5, 6, 13, 0, 1, 18, 23];

var sum = arr.reduce((a, b) => a + b);  
// 66

var even = arr.filter(v => v % 2 == 0); 
// [6, 0, 18]

var double = arr.map(v => v * 2);       
// [10, 12, 26, 0, 2, 36, 46]

// 更简明的promise链
promise.then(a => {
  // ...
}).then(b => {
  // ...
});

// 无参数箭头函数在视觉上容易分析
setTimeout( () => {
  console.log('I happen sooner');
  setTimeout( () => {
    // deeper code
    console.log('I happen later');
  }, 1);
}, 1);
```

**箭头函数也可以使用条件（三元）运算符：**

```javascript
var simple = a => a > 15 ? 15 : a;
simple(16); // 15
simple(10); // 10

let max = (a, b) => a > b ? a : b;
```

**箭头函数内定义的变量及其作用域**

```javascript
// 常规写法
var greeting = () => {let now = new Date(); return ("Good" + ((now.getHours() > 17) ? " evening." : " day."));}
greeting();          //"Good day."
console.log(now);    // ReferenceError: now is not defined 标准的let作用域

// 参数括号内定义的变量是局部变量（默认参数）
var greeting = (now=new Date()) => "Good" + (now.getHours() > 17 ? " evening." : " day.");
greeting();          //"Good day."
console.log(now);    // ReferenceError: now is not defined

// 对比：函数体内{}不使用var定义的变量是全局变量
var greeting = () => {now = new Date(); return ("Good" + ((now.getHours() > 17) ? " evening." : " day."));}
greeting();           //"Good day."
console.log(now);     // Fri Dec 22 2017 10:01:00 GMT+0800 (中国标准时间)

// 对比：函数体内{} 用var定义的变量是局部变量
var greeting = () => {var now = new Date(); return ("Good" + ((now.getHours() > 17) ? " evening." : " day."));}
greeting(); //"Good day."
console.log(now);    // ReferenceError: now is not defined
```

**箭头函数也可以使用闭包：**

```javascript
// 标准的闭包函数
function A(){
      var i=0;
      return function b(){
              return (++i);
      };
};

var v=A();
v();    //1
v();    //2


//箭头函数体的闭包（ i=0 是默认参数）
var Add = (i=0) => {return (() => (++i) )};
var v = Add();
v();           //1
v();           //2

//因为仅有一个返回，return 及括号（）也可以省略
var Add = (i=0)=> ()=> (++i);
```

** 箭头函数递归**

```javascript
var fact = (x) => ( x==0 ?  1 : x*fact(x-1) );
fact(5);       // 120
```

** 浏览器兼容**  
![](/assets/屏幕快照 2017-12-31 19.43.20.png)![](/assets/屏幕快照 2017-12-31 19.43.39.png)

