# python
### 数组及操作方法
* 数组就是一组数据的集合，javascript中，数组里面的数据可以是不同类型的
```
#定义数组的方法
//对象的实例创建
var aList = new Array(1,2,3);
//直接量创建（常用）
var aList2 = [1,2,3,"asd"];
# 操作数据中数据的方法
var aList = [1,2,3,4]
1.获取数组的长度：aList.length
    - alert（aList.length）; //弹出4
2.用下标操作数组的某个数据：aList[0];
    - alert(aList[0]); //弹出1
3.join()将数组成员通过一个分割符合并成字符串
    - alert（aList.join("-")）; //弹出 1-2-3-4
4.push()和pop()从数组最后增加成员或删除成员
    - aList.push(5);
    - alert(aList); //弹出1,2,3,4,5
    - aList.pop();
    - alert(aList); //弹出1,2,3
5.reverse()将数组反转
    - aList.reverse();
    - alert(aList); //弹出4，3，2，1
6.indexOf()返回数组中元素第一次出现的索引值
    - alert(aList.indexOf(1)); //弹出0
7.splice()在数组中增加或删除成员
    #（位置，数量，添加的成员）
    - aList.splice(2,1,7,8,9); //从第2个元素开始，
    - alert（aList）; //弹出1，2，7，8，9，4

# 数组去重
var aList = [1,2,3,4,4,3,2,1,2,3,4,5,6,5,5,3,3,4,2,1];

var aList2 = [];

for(var i=0;i<aList.length;i++)
{
    if(aList.indexOf(aList[i])==i)
    {
        aList2.push(aList[i]);
    }
}

alert(aList2);
```
* 多维数组指的是数组的成员也是数组的数组。
```
var aList = [[1,2,3];["a", "b", "c"]];
alert(aList[0][1]); //弹出2；
```
### 循环语句
* 程序中进行有规律的重复操作，需要用的循环语句
```
# while
初始值；
while(条件)｛
        命令;
        增量;
｝    
# for
for(初始值;条件;增量)｛
        命令;
｝
```
### 字符串处理方法

1.字符串合并操作："+"

2.parselnt()将数字字符转化为整数（去掉小数部分
）
3.parseFloat()将数字字符串转化为小数

4.split()把一个字符串分隔成字符串组成的数组

5.indexOf()查找字符串是否含有某字符

6.substring()截取字符串用法：substring(start, end)(不包括end)

### 调式程序的方法
1.alert(打断程序的运行)
2.console.log(控制台输出)
3.document.title(网页标题输出一般不用)
### 定时器
* 定时器在javascript中的作用
1.定时调用函数
2.制作动画
```
#定时器类型及语法
/*
定时器：
# 单次定时器
setTimeout 只执行一次的定时器
clearTimeout 关闭只执行一次的定时器
# 多次循环定时器
setInterval 反复执行的定时器
clearInterval 关闭反复执行的定时器
*/
var time1 = setTimeout(myalert,2000);
var time2 = setInterval(myalert,2000);
/*
# 关闭定时器
clearTimeout(time1);
clearInterval(time2);
# 清空定时器，让定时器不保存任何命令，释放浏览器资源
time1 = null;
time2 = null;
*/
# 执行命令：
1.匿名函数function(){};
setTimeout(function(){
        alert("匿名函数单次定时")
}, 2000)
2.自定义函数形式（*** 只写自定义函数名称，不是放调用式）
setInterval(myalert, 2000);
function myalert(){
        alert("自定义函数多次定时");
}
```
### 变量作用域

* 变量作用域指的是变量的作用范围，javascript中的变量分为全局变量和局部变量

1.全局变量：在函数之外定义的变量，为整个页面公用，函数内部外部都可以访问

2.局部变量：在函数内部定义的变量，只能在定义该变量的函数内部访问，外部无法访问

3.函数体里面声明局部变量的话，切记一定要带var，否则就是代表声明全局变量

### 封闭函数
* 封闭函数是javascript中匿名函数的另外一种写法，创建一个一开始就执行而不用命名的函数
```
#一般定义的函数和执行函数
function maalert(){
        alert("hello!");
};
myalert();
#封闭函数
(function(){
        alert("hello!");
})();
#定义前加上"~"和"!"等符号来定义匿名函数
！function(){
        alert("hello!");
}()
```
* 封闭函数的作用：可以创造一个独立的空间，在封闭函数内定义的变量和函数不会影响外部同名的函数和变量，可以避免命名冲突，在页面上引入多个js文件时，用这种方式添加js文件比较安全


