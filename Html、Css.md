# python
### 总结

```
#初学者写h+c的步骤：
分析（标签布局—行div —>和父级一样大div，包含一个版心div） —>填内容 —> 分析（列） —> 选合适标签 —> 列一般都要浮动 —> 调上下左右的位置 —> 调细节（文字的样式）

# css 初始化

<style>
# 清除标签的margin和padding
bockquote, body, button, dd, dl, dt, fieldset, form, h1, h2, h3, h4, h5, h6, hr, input, legend, li, ol, p, pre, td, textares, th, ul{ margin: 0; padding: 0;}
# 清除列表前的符号为无
ul,ol{list-style:none;}
# 设置文字默认样式和超链接的默认样式
body{font-size: 12px; color: #333; font-family: "微软雅黑";}
a{text-decoration: none; color: #333;}
#伪类状态
a:hover{color: red;}
</style>


# css引入方式

# 1.内联式（行内式）
<div style="width:100px; height:100px; background:red">....</div>
# 2.嵌入式
<style type="text/css">
    div{width:100px; height:100px; backgroud:red}
</style>
# 3.外链式（项目用）
<link rel="stylesheet" type="text/css" href= "css/main.css">
```

### 常用标签

```

# 标题(1~6级)
<h1></h1>
# 段落
<p></p>
# 布局
<div></div>
# 存放特殊效果文字和小图片
<span></span>
# 换行
<br>
# 加载图片 alt:盲人读屏软件
<img src="图片地址" alt="替换文本">
# 超链接（target重新打开一个窗口）默认文字蓝色，有下划线（"#"返回顶部，"##"不支持ie，"###"不挑转）
<a href="网址" target="_blank"></a>
# 空格
&nbsp;
# 注释
<!-- -->

<b>加粗</b>
<i>倾斜</i>
<u>下挂线</u>
<s>删除线</s>
# 强调语义的好处：seo喜欢把搜索关键字放到带有强调语义的标签里面，方便优先抓取
# 带有强调语义
<strong>加粗</strong>
<em>倾斜</em>
<ins>下划线</ins>
<del>删除线</del>
# 无序
<ul>
    <li>...</li>
</ul>
# 有序
<ol>
    <li>...</li>
</ol>
# 项目列表（自定义列表）
<dl>
    <dt>项目标题...</dt>
    <dd>项目描述...</dd>
</dl>

# 表单

# <form>标签 定义整体的表单区域
    - action属性定义表单数据提交地址
    - method属性定义表单提交的方式，一般有"get"方式"post"方式
# <lable>标签 为表单元素定义文字标注
# <input>标签 定义通用的表单元素
    - type属性 （默认提示文字placeholder）
            - type="text"定义单行文本输入框 
            - type="password"定义有密码输入框
            - type="radio"定义单选框 (单选功能：添加name属性，且取值完全相同；默认chenked 或 checked= "checked"; 扩大触发范围label标签包裹，保证label身上for属性和input身上的id属性一样)
            - type= "checkbox"定义复选框
            - type="file"定义上传文件
            - type="submit" 定义提交按钮
            - type= "reset" 定义重置按钮
            - type= "button" 定义一个普通按钮
    - value属性 定义表单元素的值
    - name属性 定义表单元素的名称，此名称是提交数据时的键名
# <textarea>标签 定义多行文本输入框(可以用css设置resize:none取消拖拽)
# <select>标签 定义下拉表单元素
    - <option>标签 与<select>标签配合，定义下拉表单元素中的选项(默认selected)
```

### 常用属性

```

- width设置元素（标签）的宽度
- height设置元素（标签）的高度
- background设置元素背景色或者背景图片
- border 设置元素四周的边框     
-- solid 实线
-- dashed 虚线
-- dotted 点线
-- border-top设置顶边边框
-- border-left设置左边边框
-- border-right设置右边边框
-- border-bottom设置底边边框
-- border-radius 设置圆角半径（正圆50%是最大值）

- padding设置元素包含的内容和元素边框的距离，也叫内边距
    -- 默认顺时针取值（4个值：上，右， 下，左； 3个值：上，左右，下；2个值： 上下，左右）
- margin设置元素和外边界的距离，也叫外边距
    -- auto (居中条件：1.盒子必须有width;2.标签必须是换行的标签)

- float设置元素浮动 ，浮动可以让块元素排列在一行，浮动分为左浮动和右浮动

- color设置文字的颜色
- font-size设置文字的大小
- font-family设置文字的字体（一般会是宋体或微软雅黑）
- font-weight设置文字是否加粗
- line-height设置文字的行高
- text-decoration设置文字的下划线
- text-align设置文字水平对齐
- text-indext设置文字首行缩进
- font-style设置字体是否倾斜
- font 同时设置文字的几个属性，各个属性之间用空格隔开，写的顺序有兼容问题
    -- font：是否加粗 是否倾斜 字号(必填项)/行高 字体(必填项)； -- 一定按顺序书写

- list-style设置列表中的小圆点， 一般把它设为无

- outline 设置input框获得焦点时，是否显示凸显的框线，一般设置为没有
    - outline: none;

- background-image 设置背景图片地址
- background-position 设置背景图片的位置
- background-color 设置背景颜色
可以将上面的属性设置用background属性合并成一句："background:url(bgimage.gif) left center no-repeat #00FF00"
```
### 选择器

```
# 1.标签选择器
div{color:red;}
# 2.类选择器
.box{color:red;}
# 3.层级选择器（有空格）
.box span{color:red;}
# 4.id选择器
#box{color:red;}
# 5.组选择器
.box,.bol,.bll{color:red;}
# 6.指定标签选择器
p.box{color:red;}
........................
<div class="box">...</div>
<p>...</p>
<p class= "box">....</p>

# 7.伪类及伪元素选择器(不管after还是before都必须有content属性)
## 伪类鼠标悬浮在元素上的状态
.box:hover{color:red}
## 伪元素
.box2:before{content:"行首文字"；}
.box3:after{content:"行尾文字"；}
```
### 盒子、浮动、定位等

```
## 盒子真实尺寸
- 盒子宽度 = width + padding左右 + border左右
- 盒子高度 = height + padding上下 + border上下
- 或设置(自动计算) box-sizing: border-box;
## 设置不浮动的元素相对于父级水平剧中
    - margin: x auto;
## 垂直外边距合并：当两个不浮动的元素，他们的垂直外边距相遇时，它们将形成一个外边距。合并后的外边距的高度等于两个发生合并的外边距的高度中的较大者
## margin-top塌陷：在两个不浮动的盒子嵌套的时候，内部的盒子设置的margin-top会加到外边的盒子上，导致内部的盒子margin-top设置失败，解决方法如下：
        - 1.外部盒子设置一个边框
        - 2.外部盒子设置voerflow:hidden
        - 3.使用伪元素类
        .clearfix:before{
                content: "";
                display: table;
        }

# 块元素特性:块元素，也可以称为行元素，布局中常用的标签
- 支持全部的样式
- 如果没有设置宽度， 默认的宽度为父级宽度100%
- 盒子占据一行、即使设置了宽度
# 内联元素特性：内联元素，也可以称为行内元素，布局中长用的标签
- 不支持宽、高、margin上下、padding上下
- 宽高由内容决定
- 盒子并在一行
- 代码换行，盒子之间会产生间距
- 子元素是内联元素，父元素可以用text-align属性设置子元素水平对齐方式
# 内联块元素特性
- 支持全部样式
- 如果没有设置宽高，宽高由内容决定
- 盒子并在一行
- 代码换行，盒子默认会产生间距
- 子元素是内联块元素，父元素可以用text-align属性设置子元素水平对齐方式
# 元素转换 display属性：用来设置元素的类型及隐藏的
1.none 元素隐藏且不占位置
2.block元素以块元素显示
3.inline元素以内联元素显示（宽高不生效）
4.inline-block原元素以内联块元素显示（宽高生效）

# overflow属性设置（元素溢出）
1. visible默认值。内容不会被修剪，会呈现在元素框外
2. hidden内容会被修剪，并且其余内容是不可见的，此属性还有清除浮动，清除margin-top塌陷的功能
3. scroll内容会被修剪，但是浏览器会显示滚动条以便查看其余的内容
4. auto如果内容被修剪，则浏览器会显示滚动条以便查看其余的内容

# 浮动：让块级标签完美的没有间距的在一行共存

# 浮动的特性
1.浮动元素有左浮动（float:left）和右浮动（float:right）
2.浮动的元素会向左或向右浮动，碰到父元素边界、其他元素才停下来
3.相邻浮动的块元素可以并在一行，超出父级宽度就换行
4.浮动让行内元素或块元素转化为有浮动特型的行内元素（此时不会有行内块元素间隙问题）
5.父元素如果没有设置尺寸（一般是高度不设置），父元素内整体浮动的子元素无法撑开父元素，父元素需要清除浮动
# 清除浮动
# 1.不支持低版本
.clearfix:after,.clearfix:before{
        content: "";
        display: table;
        # clear清除浮动的影响
        #clear:both,left.right
        clear: both;
}
# 2.支持低版本
.clearfix:after,.clearfix:before{
        content: "";
        height:0;
        #控制标签隐藏
        visibility:hidden;
        display:block;
        clear:both;
        # 让ie低版本支持伪元素        
        zoom:1;      
}
# 3.设置父属性（超出父级的内容隐藏：检查功能）
.con2{...overflow:hidden}
# 4.空div
<div class="con2 clearfix"></div>

# 定位
- 文档流：是指盒子按照html标签编写的顺序依次从上到下，从左到右排列，块元素占一行，行内元素在一行之内从左到右排列，先写的先排列，后写的排在后面，每个盒子都占据自己的位置
# 使用position属性来设置元素定位类型
- relative生成相对定位元素，元素所占据的文档流的位置保留，元素本身相对自身原位置进行偏移（1.不改变元素类型；2.参照物是自己;3.占位）
- absolute生成绝对定位元素，元素脱离文档流，不占据文档流的位置，可以理解为漂浮在文档流的上方，相对于一个设置了定位的父级元素进行定位，如果找不到，则相对于body元素进行定位（1.改元素类型具备inline-block的特点；2.参照物默认是浏览器；3.完全脱离标准流）
- fixed生成固定定位元素，元素脱离文档流，不占据文档流位置，可以理解为漂浮在文档流的上方，相对于浏览器窗口进行定位（1.改变元素类型具备行内块特点；2.参照物就是浏览器；3.完全脱离标准流）
- static默认值，没有定位，元素出现在正常的文档流中，相当于取消定位属性或者不设置定位属性
- 用left、right、top或者bottom来设置相对于参照元素的偏移值
- 可以用z-index属性来设置元素层级
- 绝对定位和固定定位的块元素和行内元素会自动转化为行内块元素

# 权重：样式的优先级
# 权重等级
1.！important,加在样式属性值后，权重值为10000
2.内联样式，如：style= "", 权重值为1000
3.ID选择器，如：#content,权重值为100
4.类，伪类，如：content、：hover权重值为10
5.标签选择器，如：div、p权重值为1

# 表格
1.< table >标签：声明一个表格
2.< tr >标签：定义表格中的一行
3.< td >和< th >标签：定义一行中的一个单元格，td代表普通单元格，th表示表头单元格
- colspan 设置单元格水平合并，设置值是数值
- rowspan 设置单元格垂直合并，设置值是数值
# 表格相关样式属性
- border-collapse 设置表格的边线合并，如：boeder-collapse:collapse

# 雪碧图
# 作用：降低服务器被请求次数
# 做法：background 核心技术 背景图的灵活运用
- background:url(图片地址) no-repeat 20px -440px;
```

### 常用图片格式

```

1.psd：photoshop的专用格式
优点：完整保存图像的信息，包括未压缩的图像数据、图层、透明等信息，方便图像的编辑
缺点：应用范围窄，图片容量相对比较大
2.jpg:网页制作及日常使用最普遍的图像格式
优点：图像压缩效率高，图像容量相对最小
缺点：有损压缩，图像会丢失数据而失真，不支持透明背景，不能制作成动画
3.gif：制作网页小动画的常用图像格式
优点：无损压缩，图像容量小、可以制作成动画、支持透明背景
缺点：图像色彩范围最多只有256色，不能保存色彩丰富的图像，不支持透明，透明图像边缘有哦锯齿
4.png:网页制作及日常使用比较普遍的图像格式
优点：无损压缩，图像容量小、支持透明背景和半透明色彩、透明图像的边缘光滑
缺点：不能制作成动画
* 总结：1.使用大幅画图片时，如果要使用不透明背景的图片，就使用jpg图片；如果要使用透明或者半透明背景的图片，就是有那个png图片。2.使用小幅画图片或者图标图片时，使用png图片；如果图片是动画，可以使用gif
```
