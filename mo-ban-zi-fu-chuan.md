#### 模板字符串
>模板字面量/Template literals 是允许嵌入表达式的字符串字面量。你可以使用多行字符串和字符串插值功能。它们在ES2015规范的先前版本中被称为“模板字符串/template strings”。

**语法**

```javaScript
`string text`

`string text line 1
 string text line 2`

`string text ${expression} string text`

tag `string text ${expression} string text`
```
>Note: 模板字面量也可以使用三元运算符( condition ?  true : false ) 和  嵌套 nested！  


**描述**
+ 模板字符串使用反引号 (` `) 来代替普通字符串中的用双引号和单引号。
+ 模板字符串可以包含特定语法(${expression})的占位符。
+ 占位符中的表达式和周围的文本会一起传递给一个默认函数，该函数负责将所有的部分连接起来，如果一个模板字符串由表达式开头，则该字符串被称为带标签的模板字符串，该表达式通常是一个函数，它会在模板字符串处理后被调用，在输出最终结果前，你都可以通过该函数来对模板字符串进行操作处理。在模版字符串内使用反引号（`）时，需要在它前面加转义符（\）。

```javaScript
`\`` === "`" // --> true
```

**多行字符串**
在新行中插入的任何字符都是模板字符串中的一部分，使用普通字符串，你可以通过以下的方式获得多行字符串：

```javaScript
console.log("string text line 1\n\
string text line 2");
// "string text line 1
// string text line 2"
```

要获得同样效果的多行字符串，只需使用如下代码：

```javaScript
console.log(`string text line 1
string text line 2`);
// "string text line 1
// string text line 2"

```
**表达式插补**
在普通字符串中嵌入表达式，必须使用如下语法：

```javaScript
var a = 5;
var b = 10;
console.log("Fifteen is " + (a + b) + " and\nnot " + (2 * a + b) + ".");
// "Fifteen is 15 and
// not 20."

```
现在通过模板字符串，我们可以使用一种更优雅的方式来表示：

```javaScript
var a = 5;
var b = 10;
console.log(`Fifteen is ${a + b} and\nnot ${2 * a + b}.`);
// "Fifteen is 15 and
// not 20."

```
**带标签的模板字符串**
更高级的形式的模板字面值被标记模板文本。
标记使您可以分析模板文本功能。
标记功能的第一个参数包含一个字符串值的数组。
其余的参数是相关的表达式。
最后，你的函数可以返回处理好的的字符串 （或者它可以返回完全不同的东西 , 如下例所述）。
用于该标记的函数的名称可以被命名为任何你想要的东西。

```javaScript
var person = 'Mike';
var age = 28;

function myTag(strings, personExp, ageExp) {

  var str0 = strings[0]; // "that "
  var str1 = strings[1]; // " is a "

  // 在技术上,有一个字符串在
  // 最终的表达式 (在我们的例子中)的后面,
  // 但它是空的(""), 所以被忽略.
  // var str2 = strings[2];

  var ageStr;
  
  if (ageExp > 60){
    ageStr = 'old person';
  } else {
    ageStr = 'young person';
  }

  return str0 + personExp + str1 + ageStr;

}

var output = myTag`that ${ person } is a ${ age }`;

console.log(output);    // that Mike is a young person
```

```javaScript
//show函数采用rest参数的写法如下：

let name = '张三',

    age = 20,

    message = show`我来给大家介绍:${name}的年龄是${age}.`;


function show(stringArr,...values){

    let output ="";

    let index = 0

    for(;index<values.length;index++){

        output += stringArr[index]+values[index];

    }

    output += stringArr[index];

    return output;

}

message;       //"我来给大家介绍:张三的年龄是20."
```

正如下面例子所展示的，标签函数并不一定需要返回一个字符串。

```javaScript
function template(strings, ...keys) {
  return (function(...values) {
    var dict = values[values.length - 1] || {};
    var result = [strings[0]];
    keys.forEach(function(key, i) {
      var value = Number.isInteger(key) ? values[key] : dict[key];
      result.push(value, strings[i + 1]);
    });
    return result.join('');
  });
}

var t1Closure = template`${0}${1}${0}!`;

t1Closure('Y', 'A');  // "YAY!" 

var t2Closure = template`${0} ${'foo'}!`;

t2Closure('Hello', {foo: 'World'});  // "Hello World!"

```

**原始字符串**
在标签函数的第一个参数中，存在一个特殊的属性raw ，我们可以通过它来访问模板字符串的原始字符串。

```javaScript
function tag(strings, ...values) {
  console.log(strings.raw[0]); 
  // "string text line 1 \\n string text line 2"
}

tag`string text line 1 \n string text line 2`;
```
另外，使用String.raw() 方法创建原始字符串和使用默认模板函数和字符串连接创建是一样的。

```javaScript
String.raw`Hi\n${2+3}!`;
```
**浏览器兼容**

![](/assets/屏幕快照 2018-01-01 02.44.43.png)
![](/assets/屏幕快照 2018-01-01 02.45.17.png)













