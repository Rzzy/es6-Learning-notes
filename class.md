### class详解
>转载：[http://www.cnblogs.com/E-WALKER/p/4796278.html](http://www.cnblogs.com/E-WALKER/p/4796278.html)

#### Overview
借助`class` 我们可以写出这样的代码:

```javaScript
class Point {
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }
    toString() {
        return '(' + this.x + ', ' + this.y + ')';
    }
}

class ColorPoint extends Point {
    constructor(x, y, color) {
        super(x, y);
        this.color = color;
    }
    toString() {
        return super.toString() + ' in ' + this.color;
    }
}

let cp = new ColorPoint(25, 8, 'green');
cp.toString(); // '(25, 8) in green'

console.log(cp instanceof ColorPoint); // true
console.log(cp instanceof Point); // true
```
#### Base classes
我们可以定义如下的`class`:

```javaScript
class Point {
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }
    toString() {
        return '(' + this.x + ', ' + this.y + ')';
    }
}
```
我们可以像使用ES5标准中的`constructor`一样实例化`class`

```javaScript
var p = new Point(25, 8);
p.toString();
// '(25, 8)'
```
实际上，`class`还是用`function`实现的，并没有为`js`创造一个全新的`class`体系。

```javaScript
typeof Point
'function'
```
但是，与function相比，它是不能直接调用的，也就是说必须得new出来

```javaScript
Point()
TypeError: Classes can’t be function-called
```
另外，它不会像`function`一样会被`hoisted`(原因是语义阶段无法解析到extends的内容)

```javaScript
foo(); // works, because `foo` is hoisted

function foo() {}

new Foo(); // ReferenceError

class Foo {}
```

```javaScript
function functionThatUsesBar() {
    new Bar();
}

functionThatUsesBar(); // ReferenceError

class Bar {}

functionThatUsesBar(); // OK
```

与函数一样，`class`的定义表达式也有两种，声明形式、表达式形式。之前用的都是声明形式，以下是表达式式的:

```javaScript
const MyClass = class Me {
    getClassName() {
        return Me.name;
    }
};
let inst = new MyClass();

console.log(inst.getClassName()); // Me

console.log(Me.name); // ReferenceError: Me is not defined
```
#### Inside the body of a class definition
`class`定义体是只能包含方法，不能包含属性的(标准定义组织认为原型链中不应包含属性)，属性被写在`constructor`中。
以下是三种会用到的方法(`constructor` 、`static method`、 `prototype method`)：

```javaScript
class Foo {
    constructor(prop) {
        this.prop = prop;
    }
    static staticMethod() {
        return 'classy';
    }
    prototypeMethod() {
        return 'prototypical';
    }
}
let foo = new Foo(123);
```
如下图(`[[Prototype]]`代表着继承关系)当对象被`new`出来，拿的是`Foo.prototype : Object`分支，从而可以调`prototype method`

![](/assets/js0401.gif)

`constructor`，这个方法本身，代表了`class`

```javaScript
Foo === Foo.prototype.constructor
// true
```
`constructor`有时被称为类构造器。相较于`ES5`，它可以调用父类的`constructor`(使用`super()`)。
`static methods`,它们归属于类本身，即类方法

```javaScript
typeof Foo.staticMethod
'function'
Foo.staticMethod()
'classy'
```
关于 `Getters` and `setters`，它们的语法如下:

```javaScript
class MyClass {
    get prop() {
        return 'getter';
    }
    set prop(value) {
        console.log('setter: '+value);
    }
}

let inst = new MyClass();

inst.prop = 123;
// setter: 123

inst.prop
//'getter'
```






















