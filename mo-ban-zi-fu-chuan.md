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





















