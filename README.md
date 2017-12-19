#UDP
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
