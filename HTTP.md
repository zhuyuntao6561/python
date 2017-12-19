
#### 网络通信过程:

* MAC地址：在设备与设备之间数据通信时用来标记收发双方（网卡的序列号）
* IP地址：在逻辑上标记一台电脑，用来指引数据包的收发方向（相当于电脑的序列号）
* 网络掩码：用来区分ip地址的网络号和主机号
* 默认网关：当需要发送的数据报包的目的ip不在本网段内时，就会发送给默认的一台电脑，称为网关
* 集线器：已过时，用来连接多台电脑，缺点：每次收发数据都进行广播（数据安全），网络会变的拥堵
* 交换机：集线器的升级版，有学习功能知道需要发送给哪台设备，根据需要进行单播、广播（ARP协议包（地址解析协议）-->根据ip地址获取对应的MAC地址）
* 路由器：连接多个不同，的网段，让他们之间可以进行收发数据，每次收到数据后，ip不变，但是MAC地址会变化
* DHCP：动态主机分配IP地址，自动给网络中的主机分配IP地址
* DNS：根据域名用来解析出IP（类似电话簿）
* http服务器：提供浏览器能够访问到的数据
* NAT：网络地址转换器，将局域网地址和公网地址进行映射

#### HTTP协议:

* 超文本传输协议（HyperText Transfer Protocol）是一种应用层协议
* HTTP是万维网的数据通信的基础
* HTML超文本标记语言，是一种用来定义网页的文本
* 作用：实现HTML网页相关资源的传送
* Elements显示网页的结构
* Network显示浏览器和服务器的通信

#### 浏览器请求和服务器相应
```
GET表示一个读取请求
失败的相应有404 Not Found：网页不存在
500 Internal Server Error:服务器内部出错
Content-Type提示相应的内容
```
#### HTTP请求步骤
```
步骤1：浏览器首先向服务器发送HTTP请求，请求包括：
    方法：GET还是POST，GET仅请求资源，POST会附带用户数据；
    路径：/full/url/path;
    域名：由Host头指定：Host:www.sina.com 以及其他相关的Header;
步骤2：服务器向浏览器返回HTTP相应，相应包括：
    相应代码：200表示成功（OK），3xx表示重定向，4xx表示客户端发送的请求有错误（比如404 Not Found）, 5xx表示服务器端处理时发生了错误（比如 503 Service Unavailable）;
    响应类型：由Content-Type指定，以及其他相关的Header;
步骤3：如果浏览器还需要继续向服务器请求其他资源，比如图片，就再次发出HTTP请求，重复1、2
```
#### HTTP格式

* 每个HTTP请求和响应都遵循相同的格式，一个HTTP包含Header和Body两部分,其中Body是可选的

#### HTTP GET请求的格式：
```
GET /path HTTP/1.1
Header1:Value1
Header2:Value2
Header3:Value3

每个Hearer一行一个，换行符是\r\n
```
#### HTTP POST请求的格式：
```
POST /path HTTP/1.1
Header1:Value1
Header2:Value2
Header3:Value3

body data goes here...


每一行换行符是\r\n
当遇到连续两个\r\n时,Header部分结束，后面的数据全部是Body
```
#### HTTP响应的格式：
```
200 OK HTTP/1.1
Header1: Value1
Header2: Value2
Header3: value3

body data goes here...

没一行换行符是\r\n
HTTP响应如果包含body,也是通过\r\n\r\n来分隔的



请注意：Bodyde 数据类型由Content-Type头来确定；当存在Content-Encoding时，Body数据是被压缩的（压缩的目的在于减少Body的大小，加快网络传输）
```

####  请求报文格式
```
请求行[方法 路径 版本\r\n]
请求头[头名称：头值\r\n]
空行[\r\n]
请求体
```
#### 响应报文格式
```
响应行[版本 响应状态]
响应头[头名称：头值]
空行
响应体
```

#### 长连接和短连接:

* TCP在真正的读写操作之前，server与client之间必须建立一个连接，当读写操作完成后，双方不再需要这个连接时它们可以释放这个连接，连接的建立通过三次握手，释放则需要四次握手，所以说每个连接的建立都是需要资源消耗和时间消耗的

#### TCP短链接（模拟一种情况）

* 1.client 向 server 发起连接请求
* 2.server 接到请求，双方建立连接
* 3.client 向 server 发送消息
* 4.server 回应 client
* 5.一次读写完成，此时双方任何一个都可以发起 close 操作

#### TCP长连接（模拟一种情况）

* 1.client 向 server 发起连接
* 2.server 接到请求，双方建立连接
* 3.client 向 server 发送消息
* 4.server 回应 client
* 5.一次读写完成，连接不关闭
* 6.后续读写操作..
* 7.长时间操作之后client发起关闭请求

#### TCP长/短连接操作过程

```
# 短连接的操作步骤：
建立连接——数据传输——关闭连接...建立连接——数据传输——关闭连接
# 长连接的操作步骤：
建立连接——数据传输...（保持连接）...数据传输——关闭连接
```

#### TCP长/短连接的优点个缺点

* 长连接可以省去较多的TCP建立和关闭的操作，减少浪费，节约时间。对于频繁请求资源的客户来说，较适用长连接。
* client与server之间的连接如果一直不关闭的话，会存在一个问题，随着客户端连接越来越多，server早晚有扛不住的时候，这时候server端需要采取一些策略，如关闭一些长时间没读写事件发生的连接，这样可以避免一些恶意连接导致server端服务受损；如果条件再允许就可以以客户端机器为颗粒度，限制每个客户端的最大长连接数，这样可以完全避免某个蛋疼的客户端连累后端服务。

* 短连接对于服务器来说管理较为简单，存在的连接都是有用的连接，不需要额外的控制手段。

* 但如果客户请求频繁，将在TCP的建立和关闭操作上浪费时间和带宽。

#### TCP长/短连接的应用场景

* 长连接多用于操作频繁，点对点的通讯，而且连接数不能太多情况.每个TCP连接都需要三次握手，这需要时间，如果每个操作都是先连接，再操作的话那么处理速度会降低很多，所以每个操作完后都不断开，再次处理时直接发送数据包就OK了，不用建立TCP连接。

* 小的WEB网站的http服务一般都用短链接，因为长连接对于服务端来说会耗费一定的资源，而像WEB网站这么频繁的成千上万甚至上亿客户端的连接用短连接会更省一些资源，如果用长连接，同时有成千上万的用户，如果每个用户都占用一个连接的话，那可想而知吧。所以并发量大，但每个用户无需频繁操作情况下需用短连好。

* 对于中大型WEB网站一般都采用长连接，好处是响应用户时间更短，用户体验更好，虽然更耗硬件资源一些，但这都不是事儿。

#### 小案例（模拟浏览器）

```

import socket

# 创建TCP连接
tcp_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# DNS解析 和  连接HTTP服务器
tcp_socket.connect(("www.itcastcpp.cn", 80))

# 组包 发送HTTP请求报文

# 请求行
request_line = "GET / HTTP/1.1\r\n"

# 请求头
request_header = "Host: www.itcastcpp.cn\r\n"
request_data = request_line + request_header + "\r\n"

# 发送请求
tcp_socket.send(request_data.encode())


# 接收响应报文
response_data = tcp_socket.recv(4096)

# 对响应报文进行解析 -- 切割
response_str_data = response_data.decode()
# print(response_data)

# '\r\n\r\n'之后的数据就是响应体数据
index = response_str_data.find("\r\n\r\n")

# 切割出的数据就是文件数据
html_data = response_str_data[index+4:]

# data_file = open("index.html", "wb")
# data_file.write(html_data.encode())
# data_file.close()
with open("index.html", "wb") as file:
    file.write(html_data.encode())

# 关闭套接字
tcp_socket.close()
```
