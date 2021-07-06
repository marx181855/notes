# socket简介

## 1、不同电脑上的进程之间如何通信

首要解决的问题是如何唯一标记一个进程，否则通信无从谈起！
在一台电脑可以通过进程号（PID）来唯一标识一个进程，但是在网络中这是行不通的。
其实TCP/IP协议族已经帮我们解决了这个问题，网络层的“IP地址”可以唯一表示网络中的主机，而传输层的”协议+端口“可以唯一标识主机中的应用进程（进程）。

这里利用ip地址，协议，端口就可以表标识网络的进程了，网络中的进程通信就可以利用这个标志与其他进程进行交互。

注意：
- 所谓进程指的是：运行的程序以及运行时用到的资源这个整体称之为进程（在讲多任务编程时进行详细讲解）
- 所谓 进程间通信指的是：运行的程序之间的数据共享



## 2、什么是socket

socket（简称套接字）是进程间通信的一种方式，它与其他进程通信的一个主要不同的是：
它能实现不同主机间的进程间通信，我们网络上各种各样的服务大多是基于Socket来完成通信的，例如我们每天浏览网页、QQ、收发email等等。

## 3、创建一个socket

使用函数 socke.socket 创建一个是socket，该函数有两个参数： 

- Address Family：可以选择AF_INET（用于 internet进程间通信）或者是AF_UNIX（同于一台机器进程间通信），实际工作中常用AF_INET
- Type：套接字类型，可以是SOCK_STREAM（流式套接字，主要用于TCP协议）或者SOCK_DGRAM（数据报套接字，主要用于UDP协议）


### 创建一个tcp.socket（tcp套接字）


```python
import socket


# 创建tcp的套接字
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)


# 这里是使用套接字的功能（省略）。。。


#不同的时候，关闭套接字
s.close()
```
### 创建一个upd.socket（udp套接字）


import socket

```python
# 创建udp的套接字
s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)


# 这里是使用套接字的功能（省略）。。。


#不同的时候，关闭套接字
s.close()
```


**说明：**

套接字使用流程与文件的使用流程很类似： 

1. 创建套接字
2. 使用套接字收发数据
3. 关闭套接字

## 基于TCP的套接字


tcp是基于链接的，必须先启动服务端，然后再启动客户端去链接服务端

### tcp服务端

```python
import socket

sk = socket.socket()
sk.bind(('127.0.0.1',8898)) #把地址绑定到套接字
sk.listen() #监听链接
conn,addr = sk.accept() #接受客户端链接
ret = conn.recv(1024) #接收客户端信息
print(ret.decode('utf-8')) #打印客户端信息
data = input("请输入要发送的数据:") #从键盘上获取数据
conn.send(data.encode('utf-8')) #向客户端发送信息
conn.close() #关闭客户端套接字
sk.close() #关闭服务器套接字(可选)
```

### tcp客户端


```python
import socket
sk = socket.socket() # 创建客户套接字
sk.connect(('127.0.0.1',8898)) # 尝试连接服务器
data = input("请输入要发送的数据：") # 从键盘上获取数据
sk.send(data.encode('utf-8'))
ret = sk.recv(1024) # 对话(发送/接收)
print(ret.decode('utf-8'))
sk.close() # 关闭客户套接字
```

### 加上链接循环与通信循环

```python
import socket
ip_port = ('127.0.0.1', 9000)  # 电话卡
BUFSIZE = 1024  # 收发消息的尺寸
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)  # 买手机
s.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
# s.setsockopt(scoket.SOL_SOCKET,SO_REUSEADDR,1) #就是它，在bind前加
s.bind(ip_port)  # 手机插卡
s.listen()  # 手机待机
while True:
    conn, addr = s.accept()  # 手机接电话
    # print(conn)
    # print(addr)
    print('接到来自%s的电话' % addr[0])
    while True:
        msg = conn.recv(BUFSIZE)  # 听消息,听话
        if len(msg) == 0:
            break
        print(msg, type(msg))
        conn.send(msg.upper())  # 发消息,说话
conn.close()  # 挂电话
s.close()  # 手机关机

```
```python
import socket

ip_port = ('127.0.0.1', 9000)
BUFSIZE = 1024
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect_ex(ip_port)  # 拨电话
while True:
    msg = input(">>>:").strip()
    if len(msg) == 0:
        continue
    s.send(msg.encode('utf-8'))  # 发消息,说话(只能发送字节类型)
    feedback = s.recv(BUFSIZE)  # 收消息,听话
    print(feedback.decode('utf-8'))
s.close()  # 挂电话
```


如果遇到[WinError 10048] 通常每个套接字地址(协议/网络地址/端口)只允许使用一次

```python
#加入一条socket配置，重用ip和端口
s.socket(AF_INET,SOCK_STREAM)
s.setsockopt(socket.SOL_SOCKET,socket.SO_REUSEADDR,1) #就是它，在bind前加
s.bind(('127.0.0.1', 9000))
```

## 基于UDP的套接字

udp是无链接的，先启动哪一端都不会报错
### udp服务端

```python
1 ss = socket()   #创建一个服务器的套接字
2 ss.bind()       #绑定服务器套接字
3 inf_loop:       #服务器无限循环
4     cs = ss.recvfrom()/ss.sendto() # 对话(接收与发送)
5 ss.close()                         # 关闭服务器套接字
```


### udp客户端
```python
cs = socket()   # 创建客户套接字
comm_loop:      # 通讯循环
    cs.sendto()/cs.recvfrom()   # 对话(发送/接收)
cs.close()                      # 关闭客户套接字
### 服务端

import socket
ip_port = ('127.0.0.1', 9001)  # 电话卡
BUFSIZE = 1024  # 收发消息的尺寸
upd_server = socket.socket(socket.AF_INET,
socket.SOCK_DGRAM)
upd_server.bind(ip_port)
while True:
    msg,addr = upd_server.recvfrom(BUFSIZE)
    print(msg,addr)
    upd_server.sendto(msg.upper(),addr)
```
### 客户端
```python
import socket
ip_port = ('127.0.0.1', 9000)  # 电话卡
BUFSIZE = 1024  # 收发消息的尺寸
upd_client = socket.socket(socket.AF_INET,socket.SOCK_DGRAM)
while True:
    msg = input(">>: ").strip()
    if not msg:
        continue
    upd_client.sendto(msg.encode("utf-8"), ip_port)
    back_msg, addr = upd_client.recvfrom(BUFSIZE)
    print(back_msg, addr)
    

​```### 


## qq聊天(由于udp无连接，所以可以同时多个客户端去跟服务端通

​```python
#服务端
import socket
ip_port=('127.0.0.1',8001)
udp_server_sock=socket.socket(socket.AF_INET,
socket.SOCK_DGRAM) #买手机
udp_server_sock.bind(ip_port)
while True:
    qq_msg,addr=udp_server_sock.recvfrom(1024)
    print('来自[%s:%s]的一条消息:%s' %(addr[0],
[1],qq_msg.decode('utf-8')))
    back_msg=input('回复消息: ').strip()
    udp_server_sock.sendto(back_msg.encode
('utf-8'),addr)


```
```python
#客户端1
import socket
BUFSIZE=1024
udp_client_socket=socket.socket(socket.AF_INET,
socket.SOCK_DGRAM)
qq_name_dic={
    '狗哥alex':('127.0.0.1',8001),
    '瞎驴':('127.0.0.1',8001),
    '一棵树':('127.0.0.1',8001),
    '武大郎':('127.0.0.1',8001),
}
while True:
    qq_name=input('请选择聊天对象: ').strip()
    while True:
        msg=input('请输入消息,回车发送: ').strip()
        if msg == 'quit':break
        if not msg or not qq_name or qq_name not
in qq_name_dic:continue
        udp_client_socket.sendto(msg.encode
('utf-8'),qq_name_dic[qq_name])
        back_msg,addr=udp_client_socket.recvfrom
(BUFSIZE)
        print('来自[%s:%s]的一条消息:%s' %(addr[0],
addr[1],back_msg.decode('utf-8')))
udp_client_socket.close()

#客户端2
import socket
BUFSIZE=1024
udp_client_socket=socket.socket(socket.AF_INET,
socket.SOCK_DGRAM)
qq_name_dic={
    '狗哥alex':('127.0.0.1',8001),
    '瞎驴':('127.0.0.1',8001),
    '一棵树':('127.0.0.1',8001),
    '武大郎':('127.0.0.1',8001),
}
while True:
    qq_name=input('请选择聊天对象: ').strip()
    while True:
        msg=input('请输入消息,回车发送: ').strip()
        if msg == 'quit':break
        if not msg or not qq_name or qq_name not
in qq_name_dic:continue
        udp_client_socket.sendto(msg.encode
('utf-8'),qq_name_dic[qq_name])
        back_msg,addr=udp_client_socket.recvfrom
(BUFSIZE)
        print('来自[%s:%s]的一条消息:%s' %(addr[0],
addr[1],back_msg.decode('utf-8')))
udp_client_socket.close()


```

## TCP 套接字 下载文件

```python
#服务端
import socket
def send_file_to_client(conn,addr):
    filename = conn.recv(1024).decode("utf-8") #接收客户端信息
    print("客户端%s需要的下载的文件%s"%addr,filename) #打印客户端信息
    file_content = None
    try:
        f = open(filename,"rb")
        file_content = f.read()
        f.close()
    except Exception as ret:
        print('没有找到文件%s'% filename)
    if file_content: #判断文件内容不为空
        conn.send(file_content) #向客户端发送信息
    
def main():
    sk = socket.socket()
    sk.setsockopt(socket.SOL_SOCKET,socket.SO_REUSEADDR,1)
    sk.bind(('127.0.0.1',9000)) #把地址绑定到套接字
    sk.listen() #监听链接
    while True:
        conn,addr = sk.accept() #接受客户端链接
        send_file_to_client(conn,addr) #发送文件
        conn.close() #关闭客户端套接字
    sk.close() #关闭服务器套接字(可选)
main()
```
```python
#客户端
from socket import *
def main():
    #创建socket
    tcp_client_socket = socket(AF_INET,SOCK_STREAM)
    #目的信息
    server_ip = input('请输入服务器ip：')
    server_port = int(input("请输入服务器port："))
    # 链接服务器
    tcp_client_socket.connect((server_ip,server_port))
    #输入需要下载的文件名
    filename = input("请输入要下载的文件名：")
    # 文件下载请求
    tcp_client_socket.send(filename.encode('utf-8'))
    # 接受对方发送过来的数据，最大接受1024个字节（1k）
    recv_data = tcp_client_socket.recv(1024)
    # 如果接收到数据再创建文件，否则不创建
    if recv_data:
        # print('接收的数据为：',recv_data.decode('utf-8'))
        with open("[接收]"+filename,'wb') as f:
            f.write(recv_data)
    else:
        print("没有找到")
    # 关闭套接字
    tcp_client_socket.close()
main()
```