# python

### 非阻塞网络IO

 * 非阻塞的特点：当没有数据来的时候不阻塞当前进程等待，而是报出一个异常 （套接字.setblocking（False））


###  IO多路复用 

  * 多路IO好处就在于单个process就可以同时处理多个网络连接的IO
  * 特点: 通过一种机制使一个进程能同时等待多个文件描述符，而这些文件描述（套接字描述符）其中的任意一个进入读就绪状态，epoll()函数就可以返回
  * epoll 只能在Linux中使用
  * EPOLLIN(可读)
  * EPOLLOUT（可写）
  * EPOLLET(ET模式)

##### 水平触发和边缘触发

  * LT（level trigger）：会在数据存在的情况下一直通知
  * ET(edge trigger): 只在数据到达的一刻通知一次

### 文件描述符

  * 文件描述符就是对进程内部所拥有文件资源的一种描述的符号，是一个无符号整数(0,1,2...)
  *  启动一个程序默认启动 标准输入、标准输出、标准错误  sock.fileno()

### web服务器-多线程

```
import socket
import threading


def request_handler(client_socket):
    """专门来处理客户端请求的函数"""
    # 接收用户请求
    recv_data = client_socket.recv(1024)
    if not recv_data:
        print("客户端已断开连接")
        client_socket.close()
        return
    # 解码数据
    recv_str_data = recv_data.decode()
    # 切割请求数据-->列表，取第0个元素  GET /index2.html HTTP/1.1\r\n
    request_line = recv_str_data.split("\r\n")[0]
    # 再次切割 取列表第一个元素 就是用户路径 /index2.html
    path_info = request_line.split(" ")[1]

    if path_info == "/":
        path_info = "/index.html"

    try:
        # # 尝试打开用户需要的文件， 不存在则抛出异常
        # f = open("./static" + path_info, "rb")
        # # 如果文件较大 容易产生隐患
        # # 读出文件数据
        # ret = f.read()
        # f.close()
        # with 语句 自动将对象的资源惊醒释放--> 上下文管理
        # 支持的数据资源有文件 socket 互斥锁等
        # static文件夹为你访问的数据（自备），与本程序放在同一目录下
        with open("./static" + path_info, "rb") as f:
            # 读出文件数据
            ret = f.read()


    except Exception as e:
        response_line = "HTTP/1.1 404 NOt Found\r\n"
        response_header = "Server: PythonServer2.0\r\n"
        response_body = "ERROR"
        response_data = response_line + response_header + "\r\n" + response_body
        client_socket.send(response_data.encode())
    else:
        # 响应行 响应头 \r\n 响应体
        # 响应行
        response_line = "HTTP/1.1 200 OK\r\n"
        # 响应头
        response_header = "Server: PythonServer1.0\r\n"
        # 响应体
        request_body = ret
        # 报文拼接
        data = (response_line + response_header + "\r\n").encode() + request_body
        # 发送
        client_socket.send(data)
    finally:
        # 关闭
        client_socket.close()


if __name__ == '__main__':
    # 创建套接字
    server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    # 地址重用（1.设置 0.取消）
    server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    # 绑定端口
    server_socket.bind(("", 8888))
    # 监听
    server_socket.listen(128)
    while True:
        # 取出一个客户端套接字用以通信
        client_socket, client_address = server_socket.accept()
        print("接收到来自客户端%s的请求" % str(client_address))
        # request_handler(client_socket)
        # 为每个客户端请求的执行都创建一个线程
        # 创建线程
        thd = threading.Thread(target=request_handler, args=(client_socket, ))
        thd.start()
```

### web服务器-多进程

```
import socket
import multiprocessing


def request_handler(client_socket):
    """专门来处理客户端请求的函数"""
    # 接收用户请求
    recv_data = client_socket.recv(1024)
    if not recv_data:
        print("客户端已断开连接")
        client_socket.close()
        return
    # 解码数据
    recv_str_data = recv_data.decode()
    # 切割请求数据-->列表，取第0个元素  GET /index2.html HTTP/1.1\r\n
    request_line = recv_str_data.split("\r\n")[0]
    # 再次切割 取列表第一个元素 就是用户路径 /index2.html
    path_info = request_line.split(" ")[1]

    if path_info == "/":
        path_info = "/index.html"

    try:
        # # 尝试打开用户需要的文件， 不存在则抛出异常
        # f = open("./static" + path_info, "rb")
        # # 如果文件较大 容易产生隐患
        # # 读出文件数据
        # ret = f.read()
        # f.close()
        # with 语句 自动将对象的资源惊醒释放--> 上下文管理
        # 支持的数据资源有文件 socket 互斥锁等      
        # static文件夹为你访问的数据（自备），与本程序放在同一目录下
        with open("./static" + path_info, "rb") as f:
            # 读出文件数据
            ret = f.read()

    except Exception as e:
        response_line = "HTTP/1.1 404 NOt Found\r\n"
        response_header = "Server: PythonServer2.0\r\n"
        response_body = "ERROR"
        response_data = response_line + response_header + "\r\n" + response_body
        client_socket.send(response_data.encode())
    else:
        # 响应行 响应头 \r\n 响应体
        # 响应行
        response_line = "HTTP/1.1 200 OK\r\n"
        # 响应头
        response_header = "Server: PythonServer1.0\r\n"
        # 响应体
        request_body = ret
        # 报文拼接
        data = (response_line + response_header + "\r\n").encode() + request_body
        # 发送
        client_socket.send(data)
    finally:
        # 关闭
        client_socket.close()


if __name__ == '__main__':
    # 创建套接字
    server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    # 地址重用（1.设置 0.取消）
    server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    # 绑定端口
    server_socket.bind(("", 8888))
    # 监听
    server_socket.listen(128)
    while True:
        # 取出一个客户端套接字用以通信
        client_socket, client_address = server_socket.accept()
        print("接收到来自客户端%s的请求" % str(client_address))
        # request_handler(client_socket)
        # 为每个客户端请求的执行都创建一个线程
        # 创建进程
        pro = multiprocessing.Process(target=request_handler, args=(client_socket,))
        pro.start()
        # 关闭在父进程的套接字，因为在子进程中使用这个套接字，而父进程中已经不需要了
        # 父进程和子进程中共有两个对象 引用底层的套接字资源 为了让套接字正常使用
        client_socket.close()
```

### web服务器-面向对象

```
import socket
import multiprocessing


class HTTPServer(object):
    def __init__(self):
        """创建服务器相关资源"""
        # 创建套接字
        server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        # 地址重用（1.设置 0.取消）
        server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
        # 绑定端口
        server_socket.bind(("", 8888))
        # 监听
        server_socket.listen(128)

        self.server_socket = server_socket

    def request_handler(self, client_socket):
        """专门来处理客户端请求的函数"""
        # 接收用户请求
        recv_data = client_socket.recv(1024)
        if not recv_data:
            print("客户端已断开连接")
            client_socket.close()
            return
        # 解码数据
        recv_str_data = recv_data.decode()
        # 切割请求数据-->列表，取第0个元素  GET /index2.html HTTP/1.1\r\n
        request_line = recv_str_data.split("\r\n")[0]
        # 再次切割 取列表第一个元素 就是用户路径 /index2.html
        path_info = request_line.split(" ")[1]

        if path_info == "/":
            path_info = "/index.html"

        try:
            # # 尝试打开用户需要的文件， 不存在则抛出异常
            # f = open("./static" + path_info, "rb")
            # # 如果文件较大 容易产生隐患
            # # 读出文件数据
            # ret = f.read()
            # f.close()
            # with 语句 自动将对象的资源惊醒释放--> 上下文管理
            # 支持的数据资源有文件 socket 互斥锁等
            # static文件夹为你访问的数据（自备），与本程序放在同一目录下
            with open("./static" + path_info, "rb") as f:
                # 读出文件数据
                ret = f.read()


        except Exception as e:
            response_line = "HTTP/1.1 404 NOt Found\r\n"
            response_header = "Server: PythonServer2.0\r\n"
            response_body = "ERROR"
            response_data = response_line + response_header + "\r\n" + response_body
            client_socket.send(response_data.encode())
        else:
            # 响应行 响应头 \r\n 响应体
            # 响应行
            response_line = "HTTP/1.1 200 OK\r\n"
            # 响应头
            response_header = "Server: PythonServer1.0\r\n"
            # 响应体
            request_body = ret
            # 报文拼接
            data = (response_line + response_header + "\r\n").encode() + request_body
            # 发送
            client_socket.send(data)
        finally:
            # 关闭
            client_socket.close()

    def start(self):
        while True:
            # 取出一个客户端套接字用以通信
            client_socket, client_address = self.server_socket.accept()
            print("接收到来自客户端%s的请求" % str(client_address))
            # request_handler(client_socket)
            # 为每个客户端请求的执行都创建一个线程
            # 创建进程
            pro = multiprocessing.Process(target=self.request_handler, args=(client_socket,))
            pro.start()
            # 关闭在父进程的套接字，因为在子进程中使用这个套接字，而父进程中已经不需要了
            # 父进程和子进程中共有两个对象 引用底层的套接字资源 为了让套接字正常使用
            client_socket.close()


if __name__ == '__main__':
    # 创建一个web服务器实例对象
    hs = HTTPServer()
    # 调用实例方法
    hs.start()

```

### web服务器-协程

```
from gevent import monkey

monkey.patch_all()
import gevent
import socket


class HTTPServer(object):
    def __init__(self):
        """创建服务器相关资源"""
        # 创建套接字
        server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        # 地址重用（1.设置 0.取消）
        server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
        # 绑定端口
        server_socket.bind(("", 8888))
        # 监听
        server_socket.listen(128)

        self.server_socket = server_socket

    def request_handler(self, client_socket):
        """专门来处理客户端请求的函数"""
        # 接收用户请求
        recv_data = client_socket.recv(1024)
        if not recv_data:
            print("客户端已断开连接")
            client_socket.close()
            return      
        # 解码数据
        recv_str_data = recv_data.decode()
        # 切割请求数据-->列表，取第0个元素  GET /index2.html HTTP/1.1\r\n
        request_line = recv_str_data.split("\r\n")[0]
        # 再次切割 取列表第一个元素 就是用户路径 /index2.html
        path_info = request_line.split(" ")[1]

        if path_info == "/":
            path_info = "/index.html"

        try:
            # # 尝试打开用户需要的文件， 不存在则抛出异常
            # f = open("./static" + path_info, "rb")
            # # 如果文件较大 容易产生隐患
            # # 读出文件数据
            # ret = f.read()
            # f.close()
            # with 语句 自动将对象的资源惊醒释放--> 上下文管理
            # 支持的数据资源有文件 socket 互斥锁等
            # static文件夹为你访问的数据（自备），与本程序放在同一目录下
            with open("./static" + path_info, "rb") as f:
                # 读出文件数据
                ret = f.read()


        except Exception as e:
            response_line = "HTTP/1.1 404 NOt Found\r\n"
            response_header = "Server: PythonServer2.0\r\n"
            response_body = "ERROR"
            response_data = response_line + response_header + "\r\n" + response_body
            client_socket.send(response_data.encode())
        else:
            # 响应行 响应头 \r\n 响应体
            # 响应行
            response_line = "HTTP/1.1 200 OK\r\n"
            # 响应头
            response_header = "Server: PythonServer1.0\r\n"
            # 响应体
            request_body = ret
            # 报文拼接
            data = (response_line + response_header + "\r\n").encode() + request_body
            # 发送
            client_socket.send(data)
        finally:
            # 关闭
            client_socket.close()

    def start(self):
        while True:
            # 取出一个客户端套接字用以通信
            client_socket, client_address = self.server_socket.accept()
            print("接收到来自客户端%s的请求" % str(client_address))
            # 创建并执行协程
            gevent.spawn(self.request_handler, client_socket)


if __name__ == '__main__':
    # 创建一个web服务器实例对象
    hs = HTTPServer()
    # 调用实例方法
    hs.start()


```

### web服务器-命令行参数控制端口

```
from gevent import monkey
monkey.patch_all()
import socket
import gevent
import sys


class HTTPServer(object):
    def __init__(self,port):
        """创建服务器相关的资源 """
        # 创建服务器套接字
        server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

        # 为了防止服务器不能立马重新使用相应的端口 设置套接字地址重用选项 1设置 0取消
        server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
        # 绑定
        server_socket.bind(('', port))

        # 监听
        server_socket.listen(128)

        self.server_socket = server_socket

    def request_handler(self, client_socket):
        """专门用来处理客户端请求的函数"""
        # 接收用户请求
        recv_data = client_socket.recv(4096)
        if not recv_data:
            print("客户端已经断开连接")
            client_socket.close()
            return  # 如果客户端已经断开连接 则不需要再执行后续代码 直接结束函数即可
        # 解码数据
        recv_str_data = recv_data.decode()

        # 切割请求数据 ----> 列表
        data_list = recv_str_data.split('\r\n')
        # print(data_list)

        # 列表 中的第0个元素就是请求行  GET /index.html HTTP/1.1
        request_line = data_list[0]
        # print(request_line)

        # 请求行中的 切割出来的列表中的第一个元素就是用户的请求路径
        path_info = request_line.split(" ")[1]
        # /index.html  /home/python/1.txt 直接使用系统的根目录存放数据容易引起数据安全问题
        # 在用户请求的路径前 加上一个指定路径  这样当用户访问的时候就会访问指定目录下的数据 防止服务器其他数据被窃取
        # ./static  + /index.html
        print(path_info)

        # 当用户只输入域名(IP) + [端口]  用户请求路径是/
        if path_info == '/':
            path_info = '/index.html'

        try:
            # # 尝试打开用户需要的文件 如果文件不存在则抛出异常
            # file = open("./static" + path_info, "rb")
            # # 如果文件比较大 容易产生隐患
            #
            # # 读出文件数据
            # file_data = file.read()
            #
            # file.close()

            # with语句 自动将对象的资源进行释放  ----> 上下文管理器
            # 支持的数据资源 有文件 socket 互斥锁等
            # static文件夹为你访问的数据（自备），与本程序放在同一目录下
            with open("./static" + path_info, "rb") as file:
                # 读出文件数据
                file_data = file.read()

        except Exception as e:
            response_line = "HTTP/1.1 404 Not Found\r\n"
            response_header = "Server: PythonServer2.0\r\n"
            response_body = "ERROR"
            response_data = response_line + response_header + "\r\n" + response_body
            client_socket.send(response_data.encode())

        else:
            # 响应行 响应头 \r\n 响应体
            # 响应行
            response_line = "HTTP/1.1 200 OK\r\n"
            # 响应头
            response_header = "Server: PythonServer2.0\r\n"
            # 响应体 就是浏览器收到的文件数据
            response_body = file_data
            # 按照HTTP响应报文格式 进行拼接
            response_data = (response_line + response_header + "\r\n").encode() + response_body

            # 发送响应报文 send不一定能够全部发送完成 sendall能够保证全部发送完成
            # client_socket.send(response_data)
            client_socket.sendall(response_data)

        finally:
            # 断开连接
            client_socket.close()

    def start(self):
        while True:
            # 取出一个客户端套接字用以通信
            client_socket, client_addr = self.server_socket.accept()
            print("接受到来自%s的连接请求" % str(client_addr))
            # 为每个客户端请求的执行 都创建一个线程
            # request_handler(client_socket)

            # 创建并且执行 协程
            gevent.spawn(self.request_handler, client_socket)


def main():
    # python3 web.py 端口号
    # sys.argv是一个列表  每个元素都是一个字符串
    if len(sys.argv) != 2:
        print("参数错误")
        return
    # print(sys.argv)
    port = sys.argv[1]
    if not port.isdigit():
        print("端口号应该是数字")
        return

    port_number = int(port)

    # 创建一个web服务器实例
    httpserver = HTTPServer(port_number)

    # 启动web服务器 的运行
    httpserver.start()


if __name__ == '__main__':
    main()




```
