# 1 字符串前加 r
## 1.1 作用：
声明后面的字符串是普通字符串，相对的，特殊字符串中含有：转义字符 \n \t 什么什么的。这样转义符就会被当成普通的字符串，而不会起作用。

## 1.2 例子：

```python
>>> print("hello world\n\n !")
hello world

 !
>>> print(r"hello world\n\n !")
hello world\n\n !
```
# 2 字符串前加 b
## 2.1 作用：
python3.x里默认的str（字符串）是unicode编码的。
b前缀代表的就是bytes ，就是把python3.x中的字符串类型转换成bytes类型。

python2.x里, 字符串就是bytes类型，因此b前缀没什么具体意义， 只是为了兼容python3.x的这种写法

Python的默认编码是ASCII编码，python3.x 的字符串是Unicode编码。

## 2.2 例子：
### 在python3中：

```python
Python 3.6.5 |Anaconda, Inc.| (default, Mar 29 2018, 13:32:41) [MSC v.1900 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import hashlib
>>> m = hashlib.md5()
>>> m.update("hello world")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: Unicode-objects must be encoded before hashing
>>> m.update(b"hello world")
>>> m.digest()
b'^\xb6;\xbb\xe0\x1e\xee\xd0\x93\xcb"\xbb\x8fZ\xcd\xc3'
>>>
```
从上面可以看到，在python3.x 哈希的对象必须要编码成字节类型，才可以哈希。python3.x 的字符串是Unicode编码。

### python2中：

```python
Python 2.7.12 (default, Nov 12 2018, 14:36:49)
[GCC 5.4.0 20160609] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> import hashlib
>>> m = hashlib.md5()
>>> m.update("hello world")
>>> m.digest()
'^\xb6;\xbb\xe0\x1e\xee\xd0\x93\xcb"\xbb\x8fZ\xcd\xc3'
>>>
```
# 3 字符串前加 u
## 3.1 作用：
后面字符串以 Unicode 格式 进行编码，一般用在中文字符串前面，防止因为源码储存格式问题，导致再次使用时出现乱码。

## 3.2 例子：
```python
u"你好，世界！"
```
# 4 字符串前加 f
## 4.1 作用：
字符串格式化（python 3.6 新增，类似于perl中的变量内插），格式化的字符串文字前缀为"f"，类似str.format()。包含由花括号包围的替换区域。替换字段是表达式，在运行时进行评估，然后使用format()协议进行格式化，和之前的format字符串格式化差不多，但是用起来更简化。

## 4.2 例子：

```python
Python 3.6.5 |Anaconda, Inc.| (default, Mar 29 2018, 13:32:41) [MSC v.1900 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> var = "python3.6"
>>> print(f"这是{var}以上版本中的新特性")
这是python3.6以上版本中的新特性
>>>
```
python3.6以下的版本是没有这个特性的，下面以Python3.5.2 为例：

```python
Python 3.5.2 |Continuum Analytics, Inc.| (default, Jul  2 2016, 17:53:06)# 
[GCC 4.4.7 20120313 (Red Hat 4.4.7-1)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> var = "python3.6"
>>> print(f"这是{var}以上版本中的新特性")
  File "<stdin>", line 1
    print(f"这是{var}以上版本中的新特性")
                            ^
SyntaxError: invalid syntax
```

