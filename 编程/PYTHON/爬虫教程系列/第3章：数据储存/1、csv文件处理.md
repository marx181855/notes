# sv文件处理
## 读取csv文件：

```python
import csv
with open('test.csv','r') as fp:
    reader = csv.reader(fp)#跳过标题行
    titles = next(reader)
    for x in reader:
        print(x)
```

这样操作，以后获取数据的时候，就要通过下表来获取数据。如果想要获取数据的时候通过标题来获取。那么可以使用 DictReader。示例代码如下：

```python
import csv
with open('test.csv','r') as fp:
    reader = csv.DictReader(fp)
    for x in reader:
        print(x['name'])
```

## 写入数据到csv文件中：
写入数据到csv文件中，需要创建一个 writer 对象，主要用到两个方法。一个是 writerow ，这个写入一行。一个是writerows，这个是写入多行。示例代码如下：

```python
import csv
headers = ['name','age','classroom']
values = {
    ('张三',19,'111'),
    ('李四',20,'222'),
    ('王五',21,'111')
}
with open('test.csv','w',newline= '',encoding='utf-8') as fp:
    writer = csv.writer(fp)
    writer.writerow(headers)
    writer.writerows(values)
```

也可以使用字典的方式把数据写入进去，这时候就需要使用DictWriter了。示例代码如下：

```python
import csv


headers = ['name','age','classroom']
values = [
{'name':'张三','age':19,'classroom':'111'},
{'name':'李四','age':20,'classroom':'222'},
{'name':'王五','age':21,'classroom':'111'}
]


with open('test1.csv','w',newline= '',encoding='utf-8') as fp:
writer = csv.DictWriter(fp,headers)
writer.writeheader()
writer.writerow({'name':'尼跟','age':23,'classroom':'333'})
writer.writerows(values)
```

