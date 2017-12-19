## 进程

* 概念：指的时运行的程序以及运行时用到的资源这个整体称之为进程
### 经典三状态：
* 就绪态：运行的条件都已经慢去，正在等待cpu执行（cpu分配时间片执行，切换到执行态）
* 执行态：cpu正在执行其功能（时间片用完就切换到就绪态）
* 等待态：在运行的代码中有需要等待某些条件（数据input()、时间sleep（））阻塞等待，如果条件满足，切换到就绪态
### 创建进程：
```
    pro = multiprocessing.Process(target= 函数名， args=(参数列表元组))
    pro.start()
```
#### 主进程阻塞等待子进程退出
```
  pro.join(time)  表示主进程最多等子进程time秒
  pro.is_alilve()  判断子进程是否存活
  pro.terminate() 直接终止子进程（信号有延时）-->.join()
  pid:当前进程的进程号(ps aux，ps -eflgrep)
  getpid()子进程的pid，getppid() 子进程的父进程的pid
```
#### 进程间是独立的地址空间，所以不共享全局变量
### 进程间通信Queue
#### 创建队列
```
q = multiprocessing.Queue(参数表示队列最大长度)
q.put()  存放数据
q.get()  取出数据
q.full()  判断是否满了
q.empty() 判断是否空了
q.qsize() 获取队列长度
```
### 进程池Pool

#### 创建进程池
```
    p = multiprocessing.Pool(参数表示进程的最大数量)
    # 添加任务
    p.apply  是阻塞添加任务，会等待添加的任务执行完成才会继续往下执行
    p.apply_async  非阻塞的任务添加，只管添加任务，不等任务结束
    # 关闭进程池
    p.close()
    p.terminate()  强制终止进程池中所有的正在执行的进程任务
    # 等待所有任务执行完成
    p.join()  保持主进程存活，等待所有子进程完成
```
#### 注意事项
* 如果需要在进程池中使用进程间通信Queue，不要使用multiprocessing.Queue()(要求是通过继承的方式创建的进程)，应该使用multiprocessing.Manager.Queue()
* .close()  .terminate()之后一般都建议更上 .join(); .join()之前一定要调用 .close() .terminate()
## 进程与线程的对比
### 定义的不同
* 进程是系统进行资源分配和调度的一个独立单位

* 线程是进程的一个实体，是CPU调度和分派的基本单位，它是比进程更小的能独立运行的基本单位，线程自己基本上不拥有系统资源，只拥有一点在运行中必不可少的资源（如程序计数器，一组寄存器和栈），但是它可与同属一个进程的其他线程共享进程所拥有的全部资源

### 区别

* 一个程序至少有一个进程，一个进程至少有一个线程
* 线程的划分尺度小于进程（资源比进度少），使得多线程程序的并发性高
* 进程在执行过程中拥有独立的内存单元，而多个线程共享内存，从而极大的提高了程序的运行效率
* 线程不能独立执行，必须依存在进程中
* 可以将进程理解为工厂中的一条流水线，而其中的线程就是这个流水线上的工人
#### 优缺点
进程和线程在使用上各有优缺点，线程执行开销小，但不利于资源的管理个保护；而进程相反
#### 进程简单的实现文件夹copy器
```
import os
import time
import multiprocessing


def copy_file(src_path, dest_path, file_name, q):
    """拷贝文件的函数"""
    f = open(src_path + "/" + file_name, "rb")
    f1 = open(dest_path + "/" + file_name, "wb")
    f1.write(f.read())
    f.close()
    f1.close()
    # 将文件名加入队列
    q.put(file_name)


if __name__ == '__main__':
    # 使用多进程 完成对指定目录下的文件拷贝
    # 1.用户输入目录-->源目录
    src_path = input("输入你要拷贝的文件名称：")
    # 2.创建一个新的目录 存放拷贝之后的文件数据
    dest_path = src_path + "[复件]"
    os.mkdir(dest_path)
    # 3.获取指定目录下的所有文件信息
    file_list = os.listdir(src_path)
    # 子进程再复制完成文件之后 将文件名写入队列 父进程从队列中取出已经完成的文件列表
    q = multiprocessing.Queue()
    # 4.传入 源目录 目的目录 文件名 使用一个子进程完成该文件的拷贝
    for file in file_list:
        pro = multiprocessing.Process(target=copy_file, args=(src_path, dest_path, file, q))
        pro.start()
    # 显示拷贝进度模块
    count = 0
    while True:
        file_name = q.get()
        count += 1
        print("\r 当前的进度%.2f %%" % ((count / len(file_list)) * 100), end="")
        time.sleep(0.6)
        if count == len(file_list):
            print("拷贝完成")
            break
```
```
# 进程小坑
import multiprocessing


def write():
    print("11")


print("123456")
if __name__ == '__main__':
    pro1 = multiprocessing.Process(target=write)
    pro1.start()


-----运行结果-----
123456
123456
11
```
