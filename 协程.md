### 迭代器
* 迭代：根据记录的前面的元素位置信息，去访问后续的元素的过程（遍历）
* 可迭代对象：通过for..in..这类语句迭代读取一条数据供我们使用的对象称之为可迭代对象；一个具备__iter__方法的对象，就是一个可迭代对象
* 可迭代的本质：提供iter(可迭代对象)获取该对象提供的一个迭代器，然后通过这个迭代器来依次获取对象中的每一个数据（iter(可迭代对象)==可迭代对象.__iter__(); next(迭代器)==迭代器.__next__()）
#### 判断对象是否是迭代对象：
```
  from collections import Iterable
  isinstance(obj, Iterable)
```
#### 迭代器：Iterator 一个实现了__iter__()方法和__next__()方法的对象就是迭代器

* for item in Iterable本质：先通过iter()函数获取可迭代对象Iterable的迭代器，然后对获取到的迭代器不断调用next()方法来获取下一个值并将其赋值给item,当遇到StopIteration的异常后循环结束

### 生成器
* 定义：生成器是一次生成一个值的特殊类型函数。可以将其视为可回复函数。调用该函数将返回一个可用于生成连续x值的生成器
#### 判断对象是否是生成器：
```
from collections import Iterator
isinstance(obj, Iterator)
```
### 生成器的2中创建方法：
* 把一个列表生成式[ ]改成（）
* 生成器函数yield

### yield关键字的作用
* 保存当前运行状态（断点），然后暂停执行，即将生成器（函数）挂器
* 将yield关键字后面表达式的值作为返回值返回，此时可以理解为起到了return的作用
#### 唤醒两种方式(让生成器从断点处继续执行，第一次在执行生成器对象的时候，必须使用next（生成器对象）)：
* next()
* send()
##### 生成器对象.send(None)==next(生成器对象)
#### send()唤醒的好处：可以在唤醒的同时向断点处传入一个附加数据
### 协程
* 概念：又称微线程，协程是python中另外一种实现多任务的方式；在一个线程中的某个函数，可以在任何地方保存当前函数的一些临时变量等信息，然后切换到另外一个函数中执行，注意不是通过调用函数的方式做到的，并且切换的次数以及什么时候再切换到原来的函数都由开发者自己确定
### 协程和线程的差异：线程切换非常耗能，协程切换只是单纯的操作CPU的上下文
#### 协程模块 yield、greenlet、gevent
#### 创建并执行协程
```
import gevent 
gr1 = gevent.spawn(指定的函数， 函数的参数)
gr2 = gevent.spawn(指定的函数， 函数的参数)  
gevent.joinall([gr1，gr2])  阻塞等待所有协程退出
```
* 猴子补丁是gevent中的模块，将程序中用到的耗时操作的代码，换为gevent中自己实现的模块
#### 在协程中实现猴子补丁
```
from gevent import monkey
monkey.patch_all()
```
#### 进程、线程、协程对比
* 进程是资源分配的单位
* 线程是操作系统调度的单位
* 进程切换需要的资源很大，效率很低
* 线程切换需要的资源一般，效率一般
* 协程切换任务的资源很小，效率高
* 多进程、多线程根据cpu核数不一样可能是并行也可能是并发。协程的本质就是使用当前进程在不同的函数代码中切换执行，可以理解为并行。协程是一个用户层面的概念，不同协程的模型实现可能是单线程，也可能是多线程
#### 协程简单实现并发下载器（爬虫原理）
```
from gevent import monkey

monkey.patch_all()  # path_all函数官方建议在第二行调用
import gevent
import urllib.request
import time


def down_html(url):
    """URL就是网址"""
    # 请求服务器 返回值是一个响应对象
    print("开始爬取%s" % url)
    response = urllib.request.urlopen(url)
    # 取出响应对象中的数据
    data = response.read()
    print("获取%s网页数据%d字节成功" % (url, len(data)))
    f = open("11.html", "wb")
    f.write(data)
    f.close()


if __name__ == '__main__':
    begin = time.time()
    # 创建并且运行协程
    gr1 = gevent.spawn(down_html, "http://www.baidu.com")
    gr2 = gevent.spawn(down_html, "http://www.jingdong.com")
    gr3 = gevent.spawn(down_html, "http://taobao.com")
    gevent.joinall([gr1, gr2, gr3])
    end = time.time()
    print("花费%.2fs" % (end - begin))
```
