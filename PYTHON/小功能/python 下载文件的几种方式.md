# 1 、一般同步下载

示例代码：



```python
import requests
import os

def downlaod(url, file_path):
    headers = {
        "User-Agent": "Mozilla/5.0 (Windows NT 10.0; WOW64; rv:68.0) Gecko/20100101 Firefox/68.0"
    }
    r = requests.get(url=url, headers=headers)
    with open(file_path, "wb") as f:
        f.write(r.content)
        f.flush()
```

# 2、 使用流式请求，requests.get方法的stream
默认情况下是stream的值为false，它会立即开始下载文件并存放到内存当中，倘若文件过大就会导致内存不足的情况，程序就会报错。
当把get函数的stream参数设置成True时，它不会立即开始下载，当你使用iter_content或iter_lines遍历内容或访问内容属性时才开始下载，需要注意一点：文件没有下载之前，它也需要保持连接。
```python
iter_content：一块一块的遍历要下载的内容
iter_lines：一行一行的遍历要下载的内容
```
使用上面两个函数下载大文件可以防止占用过多的内存，因为每次只下载小部分数据。

示例代码:

# 3 、异步下载文件

由于request的请求是阻塞式的，所以要用aiohttp模块来发起请求。

示例代码：

```python
import aiohttp
import asyncio
import os


async def handler(url, file_path):
    headers = {
        "User-Agent": "Mozilla/5.0 (Windows NT 10.0; WOW64; rv:68.0) Gecko/20100101 Firefox/68.0"
    }
    async with aiohttp.ClientSession() as session:
        r = await session.get(url=url, headers=headers)
        with open(file_path, "wb") as f:
            f.write(await r.read())
            f.flush()
            os.fsync(f.fileno())


loop = asyncio.get_event_loop()
loop.run_until_complete(handler(url, file_path))
```

# 4、 异步拆分下载文件
上面用的是一个协程下载一个文件，下面的方法是将文件分成几部分，每个部分用一个协程下载，最后再写入文件。

下面这个例子用的是流式写入，即把内容写入到磁盘里面。

```python
import aiohttp
import asyncio
import time
import os


async def consumer(queue):
    option = await queue.get()
    start = option["start"]
    end = option["end"]
    url = option["url"]
    filename = option["filename"]
    i = option["i"]

    print(f"第{i}个任务开始运行")
    async with aiohttp.ClientSession() as session:
        headers = {"Range": f"bytes={start}-{end}"}
        r = await session.get(url=url, headers=headers)
        with open(filename, "rb+") as f:
            f.seek(start)
            while True:
                chunk = await r.content.read(end - start)
                if not chunk:
                    break
                f.write(chunk)
                f.flush()
                os.fsync(f.fileno())
                print(f"第{i}个任务正在写入中ing")
        queue.task_done()
        print(f"第{i}个任务写入成功")


async def producer(url, headers, filename, queue, coro_num):
    async with aiohttp.ClientSession() as session:
        resp = await session.head(url=url, headers=headers)
        file_size = int(resp.headers["content-length"])
        # 创建一个文件
        with open(filename, "wb") as f:
            pass
        part = file_size // coro_num
        for i in range(coro_num):
            start = part * i
            if i == coro_num - 1:
                end = file_size
            else:
                end = start + part
            info = {
                "start": start,
                "end": end,
                "url": url,
                "filename": filename,
                "i": i,
            }
            queue.put_nowait(info)


async def main():
    # 需要填的有url，filename，coro_num
    url = ""
    filename = ""
    coro_num = 0
    headers = {
        "User-Agent": "Mozilla/5.0 (Windows NT 10.0; WOW64; rv:68.0) Gecko/20100101 Firefox/68.0"
    }
    queue = asyncio.Queue(coro_num)
    await producer(url, headers, filename, queue, coro_num)
    task_list = []
    for i in range(coro_num):
        task = asyncio.create_task(consumer(queue))
        task_list.append(task)
    await queue.join()
    for i in task_list:
        i.cancel()
    await asyncio.gather(*task_list)


startt = time.time()
loop = asyncio.get_event_loop()
loop.run_until_complete(main())
end = time.time() - startt
print(f"用了{end}秒")
```



# 5、注意
以上的示例都是介绍思路，程序并不健壮，健壮的程序需要加入错误捕获和错误处理。

 



