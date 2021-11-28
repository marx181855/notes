# 单进程、单线程、非堵塞实现并发web服务器

```python
import socket
import time


tcp_server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
tcp_server_socket.setsockopt(socket.SOL_SOCKET,socket.SO_REUSEADDR,1) #就是它，在bind前加
tcp_server_socket.bind(("", 7890))
tcp_server_socket.listen(128)
tcp_server_socket.setblocking(False)  # 设置套接字为非堵塞的方法


client_socket_list = list()

while True:
    time.sleep(0.5)


    try:
        new_socket, new_addr = tcp_server_socket.accept()
    except Exception as ret:
        print("---没有新的客户端到来-----")
    else:
        print("---只要没有产生异常，那么也就意味着来了一个新的客户端 - --")
        new_socket.setblocking(False)  # 设置套接字为非堵塞的方法
        client_socket_list.append(new_socket)


    for client_socket in client_socket_list:
        try:
            recv_data = client_socket.recv(1024)
        except Exception as ret:
            print("----这个客户端没有发送过来数据----")
        else:
            if recv_data:
                # 对方发送过来数据
                print("---客户端发送过来了数据----")
                print(recv_data)
            else:
                # 对方调用了啦close 导致了 recv返回
                client_socket_list.remove(client_socket)
                client_socket.close()
                print("---这个客户端已经关闭-----")
```

# gevent实现http服务器

```python
import socket
import re
import gevent
from gevent import monkey



monkey.patch_all()
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
        gevent.spawn(server_client,new_socket)
       
        
        # new_socket.close() # 协程跟线程一样，共享全局变量
    tcp_server_cocket.close()






if __name__ == "__main__":
    main()
```

