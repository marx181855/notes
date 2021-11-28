# yield
```python
import time


def task_1():
    while True:
        print("----1----")
        time.sleep(0.1)
        yield


def task_2():
    while True:
        print("----2----")
        time.sleep(0.1)
        yield


def main():
    t1 = task_1()
    t2 = task_2()

    # 先让t1运行一会，当t1中遇到yield的时候，暂停，然后执行t2，当它遇到yield的时候，再次切换到t1
    # 这样t1/t2/t1/t2的交替运行，最终实现了多任务。。。。协程

    while True:
        next(t1)
        next(t2)


if __name__ == "__main__":
    main()

````

# greenlet
为了更好使用协程来完成多任务，python中的greenlet模块对其封装，从而使得切换任务变得更加简单
安装方式 ：
```pip install greenlet```
```python
from greenlet import greenlet
import time


def test1():
    while True:
        print("----A----")
        gr2.switch()
        time.sleep(0.5)


def test2():
    while True:
        print("----B----")
        gr1.switch()
        time.sleep(0.5)


gr1 = greenlet(test1)
gr2 = greenlet(test2)


# 切换到gr1中运行
gr1.switch()
```
# gevert

greenlet已经实现了协程，但是这个还得人工切换，是不是觉得太麻烦了。python还有一个比greenlet更强大的并且能够自动切换任务的模块gevent

其原理是当一个greenlet遇到IO（指的是input output输入输出，比如网络、文件操作等）操作时，比如访问网络，就自动切换其他的greenlet，等到IO操作完成，再在适当的时候切换回来继续执行。

由于IO操作非常耗时，经常使程序处于等待状态，有了gevent为我们自动切换协程，就保证greenlet在运行，而不是等待IO

安装 ：```pip install gevent```

## 1、gevent的使用
```python
import gevent
import time


def f(n):
    for i in range(n):
        print(gevent.getcurrent(), i)
        gevent.sleep(0.5)  # 增加延时


print("----1-------")
g1 = gevent.spawn(f, 5)
print("----2-------")
g2 = gevent.spawn(f, 5)
print("----3-------")
g3 = gevent.spawn(f, 5)


g1.join()
g2.join()
g3.join()


# 有延时才会自动切换
# 协程核心，利用程序等待时间自动切换任务，不需要等任务完成后再切换
# 协程依赖线程，线程依赖进程
```
## 案例【并发下载器】
```python
from gevent import monkey
import gevent
import urllib.request


# 有耗时操作时需要哦
monkey.patch_all()


def my_download(url):
    print("GET:%s " % url)
    resp = urllib.request.urlopen(url)
    data = resp.read()
    print("%d bytes received from %s" % (len(data), url))


gevent.joinall(
    [
        gevent.spawn(my_download, "https://www.baidu.com/"),
        gevent.spawn(my_download, "http://www.sohu.com/a/306815999_740319"),
        gevent.spawn(my_download, "https://github.com/b3log/baidu-netdisk-downloaderx"),
    ]
)
```