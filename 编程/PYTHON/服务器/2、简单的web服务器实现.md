# 返回固定页面的http服务器

```python
import socket

def server_client(new_socket):  
# 为这个客户端返回数据  
# 1，接收浏览器发送过来的请求，即http请求  # GET /HTTP/1.1  # ...  
request = new_socket.recv(1024)  
print(request)

# 2，返回http格式的数据，给浏览器  
# 2.1 ，准备返回给浏览器的数据 ---header  
response = "HTTP/1.1 200 OK\r\n"  
response += "\r\n"  
# 2.2，准备发送给浏览器的数据 ----body  
response += "hahahha"  
new_socket.send(response.encode('utf-8'))

# 关闭套接字

def main():  # 用来完成整体的控制  
# 1,创建套接字  
tcp_server_cocket = socket.socket(socket.AF_INET,socket.SOCK_STREAM)

# 2，绑定  
tcp_server_cocket.bind(("",7890))

# 3,变为监听套接字  
tcp_server_cocket.listen(128)

#,4，等待新客户端的链接  
new_socket, client_addr = tcp_server_cocket.accept()

# 5，为这个客户端服务  server_client(new_socket)

if name == "main":  main()
```

# 返回用户需要的页面
```python
import socket
import re



def server_client(new_socket):
    # 为这个客户端返回数据
    # 1，接收浏览器发送过来的请求，即http请求
    # GET /HTTP/1.1
    # ...
    request = new_socket.recv(1024).decode('utf-8')
    # print("-"*50)
    # print(request)
    request_lines = request.splitlines()
    print("")
    print("="*50)
    print(request_lines)


    file_name = ""
    ret = re.match(r"[^/]+(/[^ ]*)", request_lines[0])
    if ret:
        file_name = ret.group(1)
        if file_name == "/":
            file_name = '/index.html'
        
    # 2，返回http格式的数据，给浏览器
    # 2.1 ，准备返回给浏览器的数据 ---header
    response = "HTTP/1.1 200 OK\r\n"
    response += "\r\n"
    # 2.2，准备发送给浏览器的数据 ----body
    # response += "hahahha"
    try:
        f = open('html'+file_name, 'rb')
    except:
        response = "HTTP/1.1 404 NOT FOUND\r\n"
        response += "\r\n"
        response += "------file not found"
        new_socket.send(response.encode('utf-8'))
    else:
        html_content = f.read()
        f.close()
        # 将response header发送给浏览器
        new_socket.send(response.encode("utf-8"))
        # 将response body发送给浏览器
        new_socket.send(html_content)


    # 关闭套接字
    new_socket.close()


def main():
    # 用来完成整体的控制
    # 1,创建套接字
    tcp_server_cocket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)


    # 2，绑定
    tcp_server_cocket.bind(("", 7890))


    # 3,变为监听套接字
    tcp_server_cocket.listen(128)


    # ,4，等待新客户端的链接
    new_socket, client_addr = tcp_server_cocket.accept()


    # 5，为这个客户端服务
    server_client(new_socket)




if __name__ == "__main__":
    main()
```
# web静态服务器-多进程


```python
import socket
import re
import multiprocessing




def server_client(new_socket):
    # 为这个客户端返回数据
    # 1，接收浏览器发送过来的请求，即http请求
    # GET /HTTP/1.1
    # ...
    request = new_socket.recv(1024).decode('utf-8')
    # print("-"*50)
    # print(request)
    request_lines = request.splitlines()
    print("")
    print("="*50)
    print(request_lines)


    file_name = ""
    ret = re.match(r"[^/]+(/[^ ]*)", request_lines[0])
    if ret:
        file_name = ret.group(1)
        if file_name == "/":
            file_name = '/index.html'


    # 2，返回http格式的数据，给浏览器
    # 2.1 ，准备返回给浏览器的素具 ---header
    response = "HTTP/1.1 200 OK\r\n"
    response += "\r\n"
    # 2.2，准备发送给浏览器的数据 ----body
    # response += "hahahha"]
    try:
        f = open('html'+file_name, 'rb')
    except:
        response = "HTTP/1.1 404 NOT FOUND\r\n"
        response += "\r\n"
        response += "------file not found"
        new_socket.send(response.encode('utf-8'))
    else:
        html_content = f.read()
        f.close()
        # 将response header发送给浏览器
        new_socket.send(response.encode("utf-8"))
        # 将response body发送给浏览器
        new_socket.send(html_content)


    # 关闭套接字
    new_socket.close()




def main():
    # 用来完成整体的控制
    # 1,创建套接字
    tcp_server_cocket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)


    # 2，绑定
    tcp_server_cocket.bind(("", 7890))


    # 3,变为监听套接字
    tcp_server_cocket.listen(128)


    while True:


        # ,4，等待新客户端的链接
        new_socket, client_addr = tcp_server_cocket.accept()


         # 5，为这个客户端服务
        p = multiprocessing.Process(target=server_client,args=(new_socket,))
        p.start()
        
        new_socket.close()
    tcp_server_cocket.close()


if __name__ == "__main__":
    main()

```



# web静态服务器-多线程




```python
import socket
import re
import threading


def server_client(new_socket):
    # 为这个客户端返回数据
    # 1，接收浏览器发送过来的请求，即http请求
    # GET /HTTP/1.1
    # ...
    request = new_socket.recv(1024).decode('utf-8')
    # print("-"*50)
    # print(request)
    request_lines = request.splitlines()
    print("")
    print("="*50)
    print(request_lines)


    file_name = ""
    ret = re.match(r"[^/]+(/[^ ]*)", request_lines[0])
    if ret:
        file_name = ret.group(1)
        if file_name == "/":
            file_name = '/index.html'


    # 2，返回http格式的数据，给浏览器
    # 2.1 ，准备返回给浏览器的素具 ---header
    response = "HTTP/1.1 200 OK\r\n"
    response += "\r\n"
    # 2.2，准备发送给浏览器的数据 ----body
    # response += "hahahha"]
    try:
        f = open('html'+file_name, 'rb')
    except:
        response = "HTTP/1.1 404 NOT FOUND\r\n"
        response += "\r\n"
        response += "------file not found"
        new_socket.send(response.encode('utf-8'))
    else:
        html_content = f.read()
        f.close()
        # 将response header发送给浏览器
        new_socket.send(response.encode("utf-8"))
        # 将response body发送给浏览器
        new_socket.send(html_content)


    # 关闭套接字
    new_socket.close()




def main():
    # 用来完成整体的控制
    # 1,创建套接字
    tcp_server_cocket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)


    # 2，绑定
    tcp_server_cocket.bind(("", 7890))


    # 3,变为监听套接字
    tcp_server_cocket.listen(128)


    while True:


        # ,4，等待新客户端的链接
        new_socket, client_addr = tcp_server_cocket.accept()


         # 5，为这个客户端服务
        p = threading.Thread(target=server_client,args=(new_socket,))
        p.start()
        
        # new_socket.close() # 线程并不会复制主线程的变量，所以不用close
    tcp_server_cocket.close()




if __name__ == "__main__":
    main()
```
































