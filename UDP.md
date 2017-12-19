### UDP
#### 网络
* 概念：网络就是一种辅助双方或者多方能够连接在一起的工具
* 目的：实现数据的共享和信息的传递
#### IP地址
* IP地址用来标识网络中的一台主机（由网络号和主机号组成）
* 局域网IP地址是只能在局域网内部使用IP地址，127.0.0.1代表本机回环地址，用于回路测试
* ifconfig:主要用以查看网卡的配置信息（ip地址）；ping ip地址：测试本机和目的的主机，网络是否畅通（如果通，则通；如果不通，可能通可能不通）
* DNS 域名解析--->ip地址：
#### 端口
* 概念：设备与外界通讯交流的出口
* 作用：表示主机上的一个应用程序
#### 编码和解码
```
   str (字符串类型)--->str.encode(编码方案)编码--->bytes(字节型，二进制类型)
              <-------bytes.decode(解码方案)解码<---

```
str.encode():将str字符串编码为字节型，并且通过返回值返回
bytes.decode():将字节型数据，解码为字符串类型，并通过返回值返回
###### 注意事项：编码方案和解码方案格式必须一致
###### 个人总结：设备（电脑）间数据传输要编码（encode()）,你想看到传输的数据是什么内容要解码（decode()）
#### 发送数据
* 概念：socket(简称 套接字)是最通用的进程间通信的一种方式（它能实现不同主机间的进程间通信）
* 通信前提：完成通信需要一对套接字（socket）, 网络通信的一端称为一个socket(原意为 插孔/插座)
* 本质：对底层网络协议TCP/IP的封装，并且提供了一套应用程序接口（API）
* bing作用：告诉操作系统我这应用程序使用某个戈定端口
#### UDP简单实现聊天器
```
import socket


def send_msg(udp_socket):
    """获取输入数据，并将其发送给对方"""
    msg_num = input("请输入要发送的数据：")
    dest_ip = input("请输入对方ip地址：")
    dest_port = int(input("请输入对方端口号："))
    udp_socket.sendto(msg_num.encode(), (dest_ip, dest_port))


def recv_msg(udp_socket):
    """接收并显示数据"""
    recv_msg, recv_ip = udp_socket.recvfrom(1024)
    print("接收到%s的数据：%s" % (str(recv_ip), recv_msg.decode("gbk")))


def main():
    # 创建套接字
    udp_socekt = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    # 绑定端口
    udp_socekt.bind(("", 6666))
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
        elif op_num == "2":
            recv_msg(udp_socekt)
        elif op_num == "3":
            print("欢迎下次使用")
            break
        else:
            print("输入有误！请重新输入")


if __name__ == '__main__':
    main()
```
