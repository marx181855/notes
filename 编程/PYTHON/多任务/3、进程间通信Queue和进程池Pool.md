# 进程间通信-Queue

Process之间有时候需要通信，操作系统提供了很多机制来实现多进程间的通信。

## 1、Queue的使用

可以使用multiprocessing模块的Queue实现多进程之间的数据传递，Queue本身是一个消息列队程序，首先用一个小实例来演示一下Queue的工作原理：
```python
from multiprocessing import Queue

q = Queue(3)  # 初始化一个Queue对象，最多可接受三条put消息
q.put("消息1")
q.put("消息2")
print(q.full())
q.put("消息3")
print(q.full())
# 因为消息队列已满下面的try会抛出异常，第一个try会等待2秒后再抛出异常，
# 第2个try会立刻抛出异常
try:
    q.put("消息4", True, 2)
except:
    print("消息队列已满，现有消息数量：%s" % q.qsize())
try:
    q.put_nowait("消息4")
except:
    print("消息队列已满，现有消息数量：%s" % q.qsize())
# 推荐的方式，先判断消息队列是否已满，再写入
if not q.full():
    q.put_nowait("消息4")
# 读取消息时，先判断消息队列是否为空，再读取
if not q.empty():
    for i in range(q.qsize()):
        print(q.get_nowait())

```
## 进程池Pool

当需要创建的子进程数量不多时，可以直接利用multiprocessing中Process动态生成多个进程，但如果上百甚至上千个目标，手动的去创建进程的工作量巨大，此时就可以用到multiprocessing模块提供的Pool方法。

初始化Pool，可以指定一个最大进程数，当有新的请求提交到Pool中时，如果池还没有满，那么就会创建一个新的进程用来执行该请求；但如果池中的进程数已经达到指定的最大值那么该请求就会等待，直到池中有进程结束，才会用之前的进程来执行新的任务，请看下面示例：


```python

from multiprocessing import Pool
import os, time, random


def worker(msg):
    t_start = time.time()
    print("%s开始执行，进程号为%d" % (msg, os.getpid()))
    # random.random()随机生成0~1之间的浮点数
    time.sleep(random.random() * 2)
    t_stop = time.time()
    print(msg, "执行完毕，耗时为%0.2f" % (t_stop - t_start))


po = Pool(3)  # 定义一个进程池，最大进程树3
for i in range(0, 10):
    # Pool().apply_asyns(要调用的目标,(传递给目标的参数元祖,))
    # 每次循环将会用空闲出来的子进程去调用目标
    po.apply_async(worker, (i,))
print("------start_------")
po.close()  # 关闭进程池，关闭后po不再接受新的请求
po.join()  # 等待po中所有子进程执行完成，必须放在close语句之后
print("--------end----------")


```
上面的代码在 windows下执行报错，但是在linux和unix里面执行没有报错。

具体解决方法 url：https://blog.csdn.net/qq_26442553/article/details/94595715
**下面为windows改进版本**

```python
from multiprocessing import Pool
import os
import time
import random


def worker(msg):
    t_start = time.time()
    print("%s开始执行，进程号为%d" % (msg, os.getpid()))
    # random.random()随机生成0~1之间的浮点数
    time.sleep(random.random() * 2)
    t_stop = time.time()
    print(msg, "执行完毕，耗时为%0.2f" % (t_stop - t_start))


def main():
    po = Pool(3)  # 定义一个进程池，最大进程树3
    for i in range(0, 10):
        # Pool().apply_asyns(要调用的目标,(传递给目标的参数元祖,))
        # 每次循环将会用空闲出来的子进程去调用目标
        po.apply_async(worker, (i,))
    print("------start_------")
    po.close()  # 关闭进程池，关闭后po不再接受新的请求
    po.join()  # 等待po中所有子进程执行完成，必须放在close语句之后
    print("--------end----------")


if __name__ == "__main__":
    main()
```


## 案例：多任务文件夹copy

```python
import os
import multiprocessing


def copy_file(file_name, old_folder_name, new_folder_name, q):
    # 完成文件的复制
    #  print("======模拟copy文件：从%s--->到%s 文件名是：%s" %(old_folder_name,
    # new_folder_name, file_name))
    old_f = open(old_folder_name + "/" + file_name, "rb")
    content = old_f.read()
    old_f.close()
    new_f = open(new_folder_name + "/" + file_name, "wb")
    new_f.write(content)
    new_f.close()
    # 如果拷贝完了文件，那么就向队列中写入一个消息，表示已经完成
    q.put(file_name)


def main():
    # 1，获取用户要copy的文件夹的名字
    old_folder_name = input("请输入要copy的文件夹的名字：")
    # 2，创加一个新的文件夹
    try:
        new_folder_name = old_folder_name + "[复件]"
        os.mkdir(new_folder_name)
    except:
        pass
    # 3，获取文件夹的所有待 copy 的文件名字
    file_names = os.listdir(old_folder_name)
    #  print(file_names)
    # 4，创建进程池
    po = multiprocessing.Pool(5)
    # 5,创建一个队列
    q = multiprocessing.Manager().Queue()
    # 5，向进程池中添加copy文件的任务
    for file_name in file_names:
        po.apply(copy_file, args=(file_name, old_folder_name, new_folder_name, q))
    # 复制原文件夹中的文件，到新的文件夹中的文件去
    po.close()
    #  po.join()
    all_file_num = len(file_names)
    copy_ok_num = 0
    while True:
        file_name = q.get()
        #   print("已经完成copy：%s " % file_name)
        copy_ok_num += 1
        print("\r拷贝的进度为：%.2f %% " % (copy_ok_num * 100 / all_file_num), end="")
        if copy_ok_num >= all_file_num:
            break


if __name__ == "__main__":
    main()
    
```