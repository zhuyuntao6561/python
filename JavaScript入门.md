# python

### JavaScript

* JavaScript是运行在浏览器端的脚本语言，主要解决的是前端与用户交互的问题，包括使用交互与数据交互
```
# js作用
1.制作网页的行为动作
2.表单验证
#注释
// 单行注释

/* 多行注释
    1、...
    2、...
*/
# 前端三大块
1.HTML: 页面结构（Html--结构--内容）
2.CSS: 页面表现：元素大小、颜色、位置、隐藏或显示、部分动画效果（Css--样式表现---美化）
3.JavaScript: 页面行为: 部分动画效果、页面与用户的交互、页面功能（Js--行为--页面动作）
```

### JavaScript嵌入页面的方式

```
1.行间事件（需要用户触发事件）
<input type="button" name="" onclick="alert("ok!");">
2.页面script标签嵌入
<script type="text/javascript">
    alert("ok")；
</script>
3.外部引入
<scrit type="text/javascript" src="js/index.js"></script>

```
### DOM

* 文档对象模型（Document Object Model,简称DOM),它给文档提供给了一种结构化的表示方法，可以改变文档的内容和呈现方式

```
#根标签（元素）HTML
#DOM通过自己的表现方式：把所有的html标签形成一个倒置的树状结构图（各个节点）
```
### 获取元素的方法

* 可以使用内置对象那个document 的getElementByld方法来获取页面上设置了id属性的元素，获取到一个html对象，然后将它复制给一个变量
```
#将javascript语句放到window.onload触发的函数里面，获取元素的语句会在页面加载完后才执行
<script type="text/javascript">
    #js入口函数
    #需求：保证浏览器先读取html+css，读取完之后，返回读取的js命令
    #js中小括号一般写参数或条件；大括号都是书写命令
    #当浏览器窗口加载完成后(html+css读取完成之后)，要执行大括号里面的命令
    window.onload = function(){
        #document是整个网页文档，搜索范围最大
        #getElementById--通过id名查找元素，保证页面确实有这个id
        var oDiv1 = document.geteElementById("div1");        
    }
</script>
..................
<div id ="div1">这是一个div元素</div>
```
### 操作元素属性

* 获取的页面元素，就可以对页面元素的属性进行操作，属性的操作包括属性的读和写
```
#操作元素属性
var 变量 = 元素.属性名 （读取属性）
元素.属性名 = 新属性值 （改写属性）
<script>
var oLink = document.geteElementById("link");
        oLink.href = "http://ww.baidu.com";
..............
<a href="###" id="link">超链接</a>
</script>

# 属性名在js中的写法
1. html的属性和js里面的属性写法一样
2. "class"属性写成"className"
3. "style"属性里面的属性，有横杠的改成驼峰式，比如："font-size",改成"fontSize"
4. innerHTML可以读取或者写入标签包裹的内容
```
### 变量
* JavaScript 是一种弱类型语言，javascript的变量类型由它的值来决定。定义变量需要用关键字"var"
```
# 5种基本数据类型：
1. number 数字类型
2. string 字符串类型
3. boolean 布尔类型 true 或 false
4. undefined 未定义类型， 变量声明未初始化，它的值就是undefined
5. 对象类型 null空对象类型，有值的对象类型，如果定义的变量将来准备保存对象，可以将变量初始化为null，在页面上获取不到对象，返回的值就是null
#检测数据类型
typeof(数据)
#同时定义多个变量可以用","隔开，公用一个"var" 关键字
var iNum = 45, sTr = "qwe"， sCount = "68";

# 变量、函数、属性、函数参数命名规范
1. 区分大小写
2. 第一个字符必须是字母、下划线（_）或者美元符号（$）
3. 其他字符可以是字母、下划线、美元符号或数字
```

### 函数

* 函数就是重复执行的代码片
```
# 函数的定义和执行
<script type="text/javascript">
    // 函数定义 
    function fnAlert(){
        alert(sTr);
    };
    // 调用函数
    fnAlert(); 
</script>

# 变量与函数预解析

- JavaScript 解析过程分为两个阶段， 先是编译阶段，然后执行阶段，在编译阶段会将function定义的函数提前，并且将var定义的变量声明提前，将它赋值为undefined

<script type="text/javascript">
        fnAlert();   // 弹出 hello！
        alert(iNum)；   // 弹出 undefied 说明变量不支持预解析
        function fnAlert(){
            alert("hello！")；
        };
        var iNum = 123；
</script>
```
* 函数传参: javascript的函数中可以传递参数， 参数不能设置缺省值
```
#return 关键字的作用
1.返回函数中的值或者对象
2.结束函数运行
```
### 条件语句
* 通过条件来控制程序的走向， 就需要用到条件语句
```
#条件运算符
==、===、>、>=、<、<=、!=、&&（并且）、||（或者）、！（否）
```
### 事件属性及匿名函数
* 事件属性：元素上除了有样式，id等属性外，还有事件属性,将函数名称赋值给元素事件属性，可以将事件和函数关联起来
* 匿名函数：定义的函数可以不给名称。可以将匿名函数的定义直接赋值给元素的事件属性来完成事件和函数的关联，这样可以减少函数命名，并且简化代码。函数如果做公共函数，就可以写成匿名函数形式
```
#常用的事件属性
鼠标点击事件属性（onclick）
鼠标滑过事件属性（onmouseover）
鼠标离开事件属性（onmouseout）

#*****语法：事件源.事件类型 = 匿名函数
#***事件源：操作的对象
#***事件类型：操作方法
#匿名函数：function(){}--放命令 

```


