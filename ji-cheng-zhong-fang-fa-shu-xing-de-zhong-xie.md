### 继承中方法属性的重写

```javaScript
function F(){};
var f = new F();
f.name = "cf";
f.hasOwnProperty("name");//true
F.prototype.age = 22;
f.hasOwnProperty("age");//false,age是原型对象的属性，name是F对象的属性，不是同一个。hasOwnProperty是对象的方法
F.prototype.isPrototypeOf(f);//true

//多态：编译时多态，运行时多态：方法重载、重写
//js不支持同名的方法
var o = {
run:function(){},
run:function(){},  //js同名会覆盖，run指向的是这个函数对象的地址，地址名加小括号就是这个类对象开始执行。function F(){} 既是类也是对象，new F()说明是一个类，F()函数名加小括号就是函数执行，说明是一个对象。
}

=======================================================================

function demo (a,b) {
    console.log(demo.length);//形参个数，2
    console.log(arguments.length);//实参个数，3
    console.log(arguments[0]);
    console.log(arguments[1]);
}
demo(4,5,6);

function add(){
    var total = 0;
    for (var i = arguments.length - 1; i >= 0; i--) {
        total += arguments[i];
    };
    return total;
}
console.log(add(1));
console.log(add(1,2));//可变长度


function fontSize(){
    var ele = document.getElementById("js");
    if (arguments.length == 0){
        return ele.style.fontSize;
    }else{
        ele.style.fontSize = arguments[0];

    }
}
fontSize(18);
console.log(fontSize());

function setting(){
     var ele = document.getElementById("js");
     if (typeof arguments[0] ==="object"){
         for(p in arguments[0]){//p是key,arguments[p]是value,
            ele.style[p] = arguments[0][p];
        }
    }else{
    ele.style.fontSize = arguments[0];
    ele.style.backgroundColor= arguments[1]
;    }
}
setting(18,"red");
setting({fontSize:20,backgroundColor:"green"});//js里面不能写同名的方法，所以只能够对方法的参数做判断，


==========================================================================


function demo(o){//demo是一个类，o是类的对象属性
     o.run();//调用属性的方法
}
var o = {run:function(){
    console.log("o is running...");
}};
demo(o);//类调用，java里面类不可以调用,这是跟java不一样的。


var p ={run:function(){
    console.log("p is running...");
}};
demo(p);//函数是一个类也是一个对象，函数调用对象就会执行起来。

function F(){};
var f = new F();
F.prototype.run = function(){console.log("111");}//原型区域，对象. 可以访问
f.run();//111
f.run = function(){console.log("222");};//只是给f自己加了一个方法，没有改变类的原型对象，相当于方法的重写。f.什么都是给自己对象加的
f.run();//222
F.prototype.run();//111
f.run = function(){//run指向一个对象的地址
    console.log("222");
    F.prototype.run();//重写父的，并且还要调用父的，
};
f.run();//222 , 111

=======================================================================


function Parent(){
    this.run = function(){//现在把Parent当成类看，run是一个对象的地址，
        console.log("parent is running...");
    }
}

function Child(){
     Parent.call(this);//继承父的方法,相当于父有了一个run方法，this.run = function(){console.log("parent is running...");}，但是2个方法不是同一个，只是相当于把父的属性和方法在这里写了一遍。
     var parentRun = this.run;//用this,parentRun指向run函数的地址，
     this.run = function (){//run重新指向，重写，添加同名的子类方法
         console.log("child is running...");
         parentRun();//地址名小括号就是对象的执行
     }
}

var c = new Child();//Child看成是类，c是对象
c.run();//run是函数的地址，地址小括号就是对象执行


========================================================================
function Parent(){
    this.name = "333";//只能通过 对象.name 访问
    var age = 34;//给嵌套函数使用
}
var p = new Parent();

console.log(p.name);//333
console.log(Parent.name);//Parent
console.log(p.age);//undefined, 
console.log(Parent.age);//undefined, 

Parent.aa = "aa"; //静态属性，对象. 访问不到，类. 访问得到
Parent.prototype.aaa = "asa";//原型公有区域，对象. 访问得到，类. 访问不到

console.log(p.aa);//undefined，
console.log(Parent.aa);//aa
console.log(p.aaa);//asa，
console.log(Parent.aaa);//undefined

p.zz = "zz";//只是加给了对象自己，没有加给类和类的原型
console.log(p.zz);//zz
console.log(Parent.zz);//undefined
console.log(Parent.prototype.zz)//undefin
==========================================================================

function Parent(){

}
Parent.prototype.run = function() {
    console.log("parent");
};

Child.prototype = Object.create(Parent.prototype);//继承
Child.prototype.constructor = Child;//修正
Child.super  = Parent.prototype; //给Child增加静态属性

function Child(){

}

Child.prototype.run=function(){
    console.log('child is running...');
    Child.super.run();
}

var c = new Child();
c.run();
```
1. 类里面的this.属性给对象用的，静态属性、方法给类用，什么都不加的和var给嵌套函数用，什么都不加的在window对象中。（静态属性、方法通过F.xx 添加）

2. 对象的静态属性、方法给自己用。（静态属性、方法通过 p.xx 添加）

3. 原型里面的属性、方法是给对象和原型自己用的。（通过 F.prototype.xx 添加）

```javaScript
<html>
<body>
<script type="text/javascript">
function F(){
    this.name = "yw";
    var age = 32;
    sch = 890;
}    

var f = new F();  
alert(f.name);//yw
alert(f.age);//undefined
alert(f.sch);//undefined
F.kr = "gh";
F.ss = function(){alert(123);}
alert(f.kr);//undefined
f.ss();//f.ss is not a function   
alert(F.prototype.kr);//undefined
F.prototype.ss();//F.prototype.ss is not a function



f.a = "a";
f.y = function(){alert("y");}
alert(F.a);//undefined
F.y();//F.y is not a function  
alert(F.prototype.a);//undefined
F.prototype.y();//F.prototype.y is not a function

F.prototype.o = "o";
F.prototype.oo = function(){alert("oo");}
alert(f.o);//o
f.oo();//oo
alert(F.o);//undefined
F.oo();//F.oo is not a function
</script>
</body>

</html>
```









