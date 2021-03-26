# json文件处理：

## 什么是json：

JSON(JavaScript Object Notation, JS 对象简谱) 是一种轻量级的数据交换格式。它基于 ECMAScript (欧洲计算机协会制定的js规范)的一个子集，采用完全独立于编程语言的文本格式来存储和表示数据。简洁和清晰的层次结构使得 JSON 成为理想的数据交换语言。 易于人阅读和编写，同时也易于机器解析和生成，并有效地提升网络传输效率。更多解释请见：https://baike.baidu.com/item/JSON/2462549?fr=aladdin

## json支持数据格式：

1、对象（字典）。使用花括号。
2、列表（数组）。使用方括号。
3、整形、浮点型、布尔类型还有null类型。
4、字符串类型（字符串必须使用双引号，不能用单引号）。

多个数据之间使用逗号分开。
注意：json本质上就是一个字符串。

## 字典和列表转json：

```python
# -*- coding:utf-8 -*-
import json


books = [
{
'title':'钢铁是怎么样练成的',
'price':'9.8'
},
{
'title':'红楼梦',
'price':'9.9'
}
]
json_str = json.dumps(books,ensure_ascii=False)
print(json_str)

# [{"title": "钢铁是怎么样练成的", "price": "9.8"}, {"title": "红楼梦", "price": "9.9"}]
```
因为 json 在 dump 的时候，只能存放 ascil 的字符，因此会将中文进行转义，这时候我们就可以使用 ensure_ascil = False 关闭这个特性。
在Python中，只有基本数据类型才能 转换成JSON 格式的字符串，也即： int 、float、str、list、dict、tuple。

## 将json数据直接dump到文件中：

json模块中除了dumps函数，还有一个dump函数，这个函数可以传入一个文件指针，直接将字符串 dump
到文件中。示例代码如下：

```python
# -*- coding:utf-8 -*-
import json


books = [
{
'title':'钢铁是怎么样练成的',
'price':'9.8'
},
{
'title':'红楼梦',
'price':'9.9'
}
]

with open('a.json','w',encoding='utf-8') as fp:
    json.dump(books,fp,ensure_ascii=False)
```
## 将一个json字符串load成Python对象：

```python
# -*- coding:utf-8 -*-
import json


json_str = '[{"title": "钢铁是怎么样练成的", "price": "9.8"}, {"title": "红楼梦", "price": "9.9"}]'
books = json.loads(json_str)
print(type(books))
print(books)


#<class 'list'>
#[{'title': '钢铁是怎么样练成的', 'price': '9.8'}, {'title': '红楼梦', 'price': '9.9'}]
```
## 直接从文件中读取json：

```python
# -*- coding:utf-8 -*-
import json


with open('a.json','r',encoding='utf-8') as fp:
json_str = json.load(fp)
print(json_str)

```

## dumps 和 dump:
 dumps和dump   序列化方法
dumps只完成了序列化为str，
dump必须传文件描述符，将序列化的str保存到文件中

**查看源码：**

```python
def dumps(obj, skipkeys=False, ensure_ascii=True, check_circular=True,
        allow_nan=True, cls=None, indent=None, separators=None,
        default=None, sort_keys=False, **kw):
    # Serialize ``obj`` to a JSON formatted ``str``.
    # 序列号 “obj” 数据类型 转换为 JSON格式的字符串
```


```python
def dump(obj, fp, skipkeys=False, ensure_ascii=True, check_circular=True,
        allow_nan=True, cls=None, indent=None, separators=None,
        default=None, sort_keys=False, **kw):
    """Serialize ``obj`` as a JSON formatted stream to ``fp`` (a
    ``.write()``-supporting file-like object).
     我理解为两个动作，一个动作是将”obj“转换为JSON格式的字符串，还有一个动作是将字符串写入到文件中，也就是说文件描述符fp是必须要的参数 """

```



## loads 和 load 
loads和load  反序列化方法
loads 只完成了反序列化，
load 只接收文件描述符，完成了读取文件和反序列化

 **查看源码：**

```python
def loads(s, encoding=None, cls=None, object_hook=None, parse_float=None, parse_int=None, parse_constant=None, object_pairs_hook=None, **kw):
    """Deserialize ``s`` (a ``str`` instance containing a JSON document) to a Python object.
       将包含str类型的JSON文档反序列化为一个python对象"""
```
```python
def load(fp, cls=None, object_hook=None, parse_float=None, parse_int=None, parse_constant=None, object_pairs_hook=None, **kw):
    """Deserialize ``fp`` (a ``.read()``-supporting file-like object containing a JSON document) to a Python object.
        将一个包含JSON格式数据的可读文件反序列化为一个python对象"""
````