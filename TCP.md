## TCP
* TCP协议：传输控制协议是一种面向连接的、可靠的、基于字节流的传输层通信协议
##### TCP通信面向连接需要经过创建连接、数据传送、终止连接三个步骤。
#### TCP可靠传输
* 采用发送应答机制
* 超时重传
* 错误校验
* 流量控制和阻塞管理
#### TCP与UDP的不同点
* tcp面向连接（确认有创建三次握手，连接已创建才作操作），udp面向数据报
* tcp有序数据传输
* 重发丢失的数据包
* 舍弃重复的数据包
* 无差错的数据传输
* 阻塞/流量控制
#### TCP与UDP通信模型应用
* UDP:在通信开始之前，不需要建立相关的链接，只需要发送数据即可
* TCP:在通信开始之前，一定要先建立相关的链接，才能发送数据
#### TCP客户端及服务端构建
##### 所谓的服务器端：就是提供服务的一方，⽽客⼾端，就是需要被服务的一方
```
# 客户端
1.创建socket套接字
2.使用connect连接到服务器
3.使用send/recv发送和接收信息
4.关闭socket套接字
# 服务器
1.创建socket套接字
2.bind绑定ip和端口
3.使用listen使套接字变为可以被动套接字
4.accept取出一个客户端连接用以服务
5.recv/send接收发送数据
6.关闭客户端套接字client
7.关闭server套接字（服务器一般不关闭）
```
#### tcp注意点
* tcp服务器一般情况下都需要绑定，否则客户端找不到这个服务器
* tcp客户端一般不绑定，因为是主动链接服务器，所以只要确定好服务器的ip、port等信息就好，本地客户端可以随 机
* tcp服务器中通过listen可以将socket创建出来的主动套接字变为被动的，这是做tcp服务器时必须要做的
* 当客户端需要链接服务器时，就需要使用connect进行链接，udp是不需要链接的而是直接发送，但是tcp必须先链 接，只有链接成功才能通信
* 当一个tcp客户端连接服务器时，服务器端会有1个新的套接字，这个套接字用来标记这个客户端，单独为这个客户 端服务
* listen后的套接字是被动套接字，用来接收新的客户端的连接请求的，而accept返回的新套接字是标识这个新客户端 的 
* 关闭listen后的套接字意味着被动套接字关闭了，会导致新的客户端不能够链接服务器，但是之前已经链接成功的客 户端正常通信
* 关闭accept返回的套接字意味着这个客户端已经服务完毕 
* 当客户端的套接字调用close后，服务器端会recv解阻塞，并且返回的长度为0，因此服务器可以通过返回数据的长 度来区别客户端是否已经下线；同理 当服务器断开tcp连接的时候 客户端同样也会收到0字节数据
#### 三次握手和四次挥手
##### 三次握手原理
```
 客户端（client）                                                    服务器（server）

                                          SYN seq=x
SYN_SENT(connect())   ------------------------------------->   LISTEN(listen())
                                   SYN seq=y,ACK=x+1   
    ESTABLISHED   <--------------------------------------------  SYN_RCVD  
                                    ACK=y+1     
                        -------------------------------------------------> ESTABLISHED              
```
* connect()本质就是发起与服务器的三次握手
* 调用lisent之后的套接字，才能收到客户端的请求
* listen函数的参数：不同系统意义不同；Linux2.6之后listen函数表示已完成三次握手队列常去（队列常去表示存放的客户端数量），其他系统一般表示已完成和未完成队列的总和
##### 四次挥手原理
```
 客户端（client）                                                                服务器（server）

                                  FIN seq=x+2,ACK=y+1
FIN_WAIT_1(close())------------------------------------------------->   CLOSE_WAIT
                                 ACK x+3   
FIN_WAIT_2   <----------------------------------------------------------  CLOSE_WAIT
                                FIN seq= y+1
TIME_WAIT   <-----------------------------------------------------------LAST_ACK(close())
                                ACK=y+2    
TIME_WAIT  --------------------------------------------------------------> 
```
##### 地址重用问题
```
server_socket.setsockopt(socket.SOl_SOCKET, socket.SO_REUSEADDR,1)(1.代表设置， 0.代表取消)
```
#### 协议
* 定义：解决不同种族人之间的语言沟通障碍，而做出的规定，就是协议
* tcp/ip协议（簇）：互联网协议包含了上百种协议标准，但是最重要的两个协议TCP协议和IP协议，所以大家把互联网的协议简称为TCP/IP协议（簇）
#### TCP实现客户端和服务端文件下载器
```
# 客户端
import socket
import os
# 创建tcp套接字
tcp_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# 创建和服务器的连接
ip = input("文件下载服务器ip:")
port = int(input("输入文件下载服务器对应端口："))
tcp_socket.connect((ip, port))

# 发送下载文件名
file_name = input("请输入需要下载的文件名：")
tcp_socket.send(file_name.encode("gbk"))
# 结合文件数据 --> 保存在本地写入文件-->二进制模式就是原封不动的写入
file = open("new"+file_name, "wb")
write_len = 0
while True:
    data = tcp_socket.recv(4096)
    if data:
        file.write(data)
        write_len += len(data)
    else:
        file.close()
        # 关闭套接字
        tcp_socket.close()
        if write_len > 0:
            print("文件传输完成")
        else:
            # 空文件
            os.remove("new"+file_name)
            print("文件不存在")
        break
```

```
# 服务端
import socket


def read_data(file_name):
    """获取文件内容"""
    try:
        f = open(file_name, "rb")
    except Exception as e:
        print("%s文件不存在" % file_name)
    else:
        # 文件太大回报错
        ret = f.read()
        return ret


# 创建一个服务器套接字
server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
#     设置套接字选项 地址重用                   # 重用地址 1.代表设置 0.代表取消设置
server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
# 绑定端口
server_socket.bind(("", 8888))
# 将套接字设置为被动套接字 监听
server_socket.listen(128)
while True:
    # 接收一个客户端 开始对他进行服务
    client_socket, client_address = server_socket.accept()
    print("接收到%s的请求" % str(client_address))
    # 取出文件名
    file_name = client_socket.recv(4096)
    # 根据文件名读取文件数据-->通过客户端关联的套接字发送文件数据
    file_ct = read_data(file_name.decode("gbk"))
    # 发送完成  应该主动断开客户端关联套接字
    if file_ct:
        client_socket.send(file_ct)
    # 关闭套接字
    client_socket.close()
# server_socket.close()
```
