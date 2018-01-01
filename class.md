### class详解
>转载：[http://www.cnblogs.com/E-WALKER/p/4796278.html](http://www.cnblogs.com/E-WALKER/p/4796278.html)

#### Overview
借助class 我们可以写出这样的代码:

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
我们可以定义如下的class:

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
我们可以像使用ES5标准中的constructor一样实例化class

```javaScript
var p = new Point(25, 8);
p.toString();
// '(25, 8)'
```
实际上，class还是用function实现的，并没有为js创造一个全新的class体系。

```javaScript
typeof Point
'function'
```
但是，与function相比，它是不能直接调用的，也就是说必须得new出来

```javaScript
Point()
TypeError: Classes can’t be function-called
```
另外，它不会像function一样会被hoisted(原因是语义阶段无法解析到extends的内容)

```javaScript
foo(); // works, because `foo` is hoisted

function foo() {}

new Foo(); // ReferenceError

class Foo {}
```























