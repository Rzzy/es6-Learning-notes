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


























