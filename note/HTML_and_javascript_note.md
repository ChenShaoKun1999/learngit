# HTML，CSS与JS

HTML定义了网页的内容，CSS 描述了网页的布局，JavaScript描述网页的行为

# HTML

```html
<!DOCTYPE html>   <!-- 声明是html5文档。标签对大小写不敏感，在将来版本会强制用小写标签 -->
<html>            <!-- 根元素 -->
    <head>            <!-- 包含元数据 -->
        <title>文档标题</title>
        <meta charset="utf-8">
        <base href="//www.runoob.com/images/" target="_blank">     <!-- 默认链接 -->
        <link rel="stylesheet" type="text/css" href="mystyle.css"> <!-- 外部样式 -->
    </head>

    <body>            <!-- body包含了可见的内容 -->

        <h1>一级标题</h1>
        <p>段落</p>
        <a href="http://www.google.com">超文本链接</a>
        <img src="/images/logo.png" width="258" height="39" />
        <br>  <!-- 换行。源代码中的连续空白、空行都会被当作一个空格 -->
        <hr>  <!-- 分割线 -->

        <p>字体
            <b>粗体(bold)</b>
            <i>斜体(italic)</i>
            <strong>加重，通常也被渲染为粗体</strong>
            <em>着重，常被渲染称斜体</em>
            <sub>下标</sub>
            <sup>上标</sup>
            <del>删除线</del>
        </p>
        
        <p style="color:blue;margin-left:20px;">
            内联样式。常用样式：
            background-color   背景颜色
            font-family        字体
            color              文字颜色
            font-size          文字尺寸，如20px
            text-align         对齐方式，如center
            在旧版本，有font, center, strike标签，color, bgcolor属性用来实现样式
            现在均不建议使用
        </p>
        
        <!-- 图像 -->
        <img src="boat.gif" alt="加载不出来的时候显示这段文字" width="304" height="228">
        
        <!-- 表格 -->
        <table width="500" border="1" cellpadding="10">
            <caption>标题</caption>
            <tr>
                <th colspan="2">Header 1</th>
                <!-- th表示表头，colspan表示跨列（即一个占两列），rowspan表跨行-->
            </tr>
            <tr>
                <td>row 1, cell 1</td>
                <td>row 1, cell 2</td>
            </tr>
            <tr>
                <td>row 2, cell 1</td>
                <td>row 2, cell 2</td>
            </tr>
        </table>
        
        <li>无序列表</li>
        <ol>
            <li>li套在ol里为有序列表</li>
        </ol>
        
        <div>
            块级元素，经常当作容器
        </div>
  </body>
</html>
```

# JavaScript基本用法

```html
<html>
    <head>
        <script>
            //在head中定义函数
            function myFunction()
            {
                document.getElementById("demo").innerHTML="Hello World!";
            }
        </script>
    </head>
    
    <body>
        <p id="demo">
            demonstration
        </p>
        <!-- 在body中调用函数 -->
        <button type="button" onclick="myFunction()">JS</button>
        
        <!-- 其实也可以在body中写js代码 -->
        <script>
            function funcInBody() {\*do something*\}
        </script>
        
        <!-- 也可以使用外部文件的代码 -->
        <script src="myScript.js"></script>
        
        <!-- 更通用的用法
        <some-HTML-element some-event='JavaScript 代码'>
        -->
    </body>
</html>
```

# JavaScript语法

## 数据类型与变量

```javascript
//基本类型
var x;      //undefined
x = 1.5;    //number
x = 'Joe';  //string，单引号或者双引号均可
x = true;   //boolean
x = null;   //值为null，类型不变
x = undefined;
            //值和类型都不定，null == undefined，但null !== undefined

//引用类型
var arr = new Array();  //这样定义的数组和python的差不多
var arr2 = [1, 4];
var person = {name:"John", age:14};
    //object，可以用person["name"]或person.name来引用

//typeof
typeof x;   //返回x的类型

//强制类型转换，以String为例，Number也类似
String(x);
x.toString();
```

### 字符串

```javascript
var str = 'some string';

str.length;

str.search('/regular expression/i')
    //返回子串起始位置
    //js中的正则表达式：/表达式内容/[修饰]
    //一些常用修饰：i不区分大小写，g全局匹配，m多行匹配

str.replace('pattern', 'repl')

//RegExp对象
var pattern = /regexp/
pattern.test('string')
    //测试string是否匹配pattern模式，返回boolean
```

### 变量提升

JavaScript允许先定义后声明，这种情况下，声明语句会被自动提升到定义前。但是在声明同时初始化的变量不会被提升，还有一些其他的复杂规则。因此还是得好好的先声明再使用

或者使用```"use strict"```指令，在ECMAScript5及之后版本中，此指令声明严格模式，不允许先定义再声明，同时禁止了一些不太安全的操作

## 运算符

算数运算符（+-*/%)，赋值运算符（=, +=, -=, *=, /=, %=），自增自减（++, --），条件（? :），比较（>, <, ==, >=, <=, !=）

大部分和C一致，以下列出不一样的地方

```javascript
'a' + 5    // = 'a5'，字符串间、字符串与数字间可以做加法

a === b;   //绝对等于，值和类型均相等
a !== b;   //不绝对等于，值或类型不相等
```

## 语句

```javascript
//条件执行
//if ... else if ... else，同C

//分支选择
//switch ... case ... default，同C

//for循环
//for( ;  ; )，同C

//for/in循环
//例：for(x in person) {do something}，类似python的for循环

//while循环，do/while循环
//同C

//break
//类似C，但js可以给代码块加标签，可以指定跳出任意代码块
var x = false;
label:
{
    //do something
    if (x) break label;
    //do some other things
}

//continue
//同C

//错误处理，类似python
try {/*do something*/}
catch(err) {/*发生错误之后执行这段*/}
finally {/*不论有没有捕捉到错误都会运行*/}

//抛出异常
throw exception;  //异常可以是字符串、数字、逻辑值或对象
```

### 错误处理

| 语句  |
| ----- |
| try   |
| throw |
| catch |

## 函数

用关键字function声明函数，函数内变量生存期为函数执行期间，作用域为函数内；函数外的为网页存续期间，作用域为整个网页；可以给未声明的变量赋值，它会自动称为网页的属性（可以删除）

```javascript
function func(a, b)
{
    return a*b;
}

var1 = 1;
window.var1;  // == 1
```

# 常用HTML事件

| 事件        | 含义                         |
| ----------- | ---------------------------- |
| onchange    | HTML 元素改变                |
| onclick     | 用户点击 HTML 元素           |
| onmouseover | 用户在一个HTML元素上移动鼠标 |
| onmouseout  | 用户从一个HTML元素上移开鼠标 |
| onkeydown   | 用户按下键盘按键             |
| onload      | 浏览器已完成页面的加载       |

使用例

```html
<button onclick="displayDate()">现在的时间是?</button>
```

# 一个实例

