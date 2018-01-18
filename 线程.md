## 多任务

* 概念：操作系统可以同时运行多个任务

* 并发：指的是任务数多余cpu核数，通过操作系统的各种任务调度算法，实现用多个任务在同一时间段执行（实际上总有一些任务不在执行，因为切换任务的速度相当快，看上去一起执行而已）

* 并行：指的是多核cpu情况下，多个任务的一些任务往往在同一时间点执行


## 线程

* 概念：就是一个进程内部的一条代码执行流程

* 默认存在的就是主线程，新创建出来的叫做子线程

### 创建线程

#### 使用threading模块 或继承threading.Thread

* target  指定线程执行的函数名

* args 指定函数的参数

* enumerate() 查看当前进程中的线程列表

* join() 主线程等待子线程执行完成后，才能继续往下运行

#### 多线程共享全局变量，如果多个线程同时对同一个全局变量操作，会出现资源竞争问题，从而数据结果会不正确

### 同步：就是协同步调，按预定的先后次序进行运行

### 互斥锁

* 概念：某个线程要更改共享资源时，先将其锁定，此时资源的状态为"锁定"，其他线程不能更改；直到该线程释放资源，将资源的状态变成"非锁定"，其他的线程才能再次锁定该资源。互斥锁保证了每次只有一个线程进行写入操作，从而保证了多线程情况下数据的正确性


### 创建使用互斥锁
```
# 创建锁
mutex = threading.Lock()
#  锁定
mutex.acqire()
# 释放
mutex.release()
```
### 优缺点

  优点：确保某段关键代码只能有一个线程从头到尾完整的执行

  缺点：
  * 阻止了多线程并发执行，包含锁的某段代码实际上只能以单线程模式执行，效率大大的下降了

  * 由于可以存在多个锁，不同的线程持有不同的锁，并试图获取对方持有的锁时，可能会造成死锁


### 死锁

* 概念：在线程间共享多个资源的时候，如果两个线程分别占有一部分资源并且同时等待对方的资源，就会造成死锁

### 主要原因

  * 没有正确释放

  * 资源分配不当

### 避免死锁


* 程序设计时要尽量避免

* 附加超时 时间等
### 线程实现简单的多任务聊天程序
```
import socket
import threading


def send_msg(udp_socket):
    """获取输入数据，并将其发送给对方"""
    msg_num = input("请输入要发送的数据：")
    dest_ip = input("请输入对方ip地址：")
    dest_port = int(input("请输入对方端口号："))
    udp_socket.sendto(msg_num.encode(), (dest_ip, dest_port))


def recv_msg(udp_socket):
    """循环接收并显示数据"""
    while True:
        recv_msug, recv_ip = udp_socket.recvfrom(1024)
        print("接收到%s的数据：%s" % (str(recv_ip), recv_msug.decode("gbk")))


def main():
    # 创建套接字
    udp_socekt = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    # 绑定端口
    udp_socekt.bind(("", 6666))

    # 创建一个子线程（收数据）
    recv_thd = threading.Thread(target=recv_msg, args=(udp_socekt,))
    recv_thd.start()

    while True:
        # 选择功能
        print("=" * 30)
        print("1.发送消息")
        print("2.接收消息")
        print("3.退出")
        print("=" * 30)
        op_num = input("请输入你要进行的操作：")
        if op_num == "1":
            send_msg(udp_socekt)
        # elif op_num == "2":
            # recv_msg(udp_socekt)
        elif op_num == "3":
            print("欢迎下次使用")
            break
        else:
            print("输入有误！请重新输入")


if __name__ == '__main__':
    main()
```
