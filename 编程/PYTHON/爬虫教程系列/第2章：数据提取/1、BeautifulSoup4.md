# BeautifulSoup4库

和 lxml 一样，Beautiful Soup也是一个HTML/XML 的解析器，主要的功能也是如何解析和图 HTML/XML数据。lxml 只会局部遍历，而Beautiful Soup是基于HTML DOM的，会载入整个文档，解析整个 DOM树，因此时间和内存开销都会大很多，所以性能要低于lxml。BeautifulSoup 用来解析 HTML比较简单，API非常人性化，支持CSS选择器、Python标准库中的HTML解析器，也支持lxml的XML解析器。

## **安装和文档：**
1、安装： pip install bs4 

2、中文文档：https://www.crummy.com/software/BeautifulSoup/bs4/doc.zh/

## **几大解析工具对比：**


|解析工具|解析速度|使用难度|
|:-----:|:-----:|:-----:|
|BeautifulSoup|最慢|最简单|
|lxml|快|简单|
|正则|最快|最难|

## 简单使用：


```python
from bs4 import BeautifulSoup
html = '''
<html><body><div>
    <ul>        
        <li class="item-0" id = 'key'><a href="link1.html">first item</a></li>
        <li class="item-1"><a href="link2.html">second item</a></li>
        <li class="item-inactive"><a href="link3.html">third item</a></li>
        <li class="item -1"><a href="link4.html">fourth item</a></li>
        <li class="item-0"><a href="link5.html">fifth item</a>
    </ul>
</div>
</body></html>'''
#创建Beautiful Soup对象#使用 lxml解析器来进行解析
bs = BeautifulSoup(html,"lxml")
print(bs.prettify())
```

## 基本使用语法

```python
from bs4 import BeautifulSoup


html = '''
<html><body><div>
    <ul>
        <li class="item-0" id = 'key'>这里是列表第一个<a href="link1.html">first item</a></li>
        <li class="item-1">
        <a href="link2.html">second item</a></li>
        <li class="item-inactive"><a href="link3.html">third item</a></li>
        <li class="item-1"><a href="link4.html">fourth item</a></li>
        <li class="item-0" id = 'key'><a href="link5.html">fifth item</a>
    </ul>
</div>
</body></html>
'''
#创建Beautiful Soup对象
#使用 lxml解析器来进行解析
bs = BeautifulSoup(html,"lxml")


#1、获取所有li标签
# lis = bs.find_all('li')
# for li in lis:
#     print(li)


#2、获取第二个li标签
# lis = bs.find_all('li',limit=2)
# print(lis[1])


#3、获取所有class等于item-0的li标签
# lis = bs.find_all('li',class_='item-1')
# print(lis)

#用attrs参数直接写标签属性
# list = bs.find_all('li',attrs = {'class':"item-1"})
# print(list)


#4、将所有id等于item-0,且class等于key 的li标签里取出来
# lis = bs.find_all('li',id='key',class_='item-0')
# print(lis)

# 用attrs参数直接写标签属性
# list = bs.find_all('li',attrs={"class":"item-0","id":"key"})
# print(list)


#5、获取所有a标签的href属性
# aList = bs.find_all('a')
# for a in aList:
#     #1、通过下标操作的方式
#     href = a['href']

#     print(href)
#     #2、通过attrs属性的方式
#     href= a.attrs['href']
#     print(href)


#6、获取所有的文字
lis = bs.find_all('li')
for li in lis:
    # aes = li.find_all("a")

    # #使用.string方法来提取标签里的文本，返回的是生成器
    # text = aes[0].string
    # print(text)


    #使用.strings可以获取标签内，包括子孙标签的所有的文本、空白字符和换行，返回的是生成器。
    # infos = li.strings
    # for i in infos:
    #     print(i)
    
    # 使用.strings可以获取标签内，包括子孙标签 所有非空白字符和换行的文本，返回的是生成器。
    infos = list(li.stripped_strings)
    print(infos)
    
    #使用.get_text()可以获取标签下的子孙非标签字符串，不是以列表的形式返回，普通字符串返回。
    infos = li.get_text()
    print(type(infos))

    tag = bs.ul
    #返回ul元素所有子节点的列表
    #print(tag.contents)


    #返回ul元素所有子节点的迭代器
    for child in  tag.children:
        print(child)

```


## CSS选择器

### 1.find和find_all方法：

搜索文档树一把用的比较多的就是两个方法，一个是find，一个是find_all。find方法是找到第一个满足条件的标签后就立即返回，只返回一个元素。 find_all方法就是把所有满足条件的标签都选到，然后返回回去。使用这两个方法，最常用的用法是输入name 以及attrs参数找出符合要求的标签。

```python
soup.find_all("a",attrs={'id':'link2'})
```
或者是直接传入属性的名字作为关键字参数：

```python
soup.find_all("a",id='link2')
```
### 2.select方法：

使用以上方法可以方便的找出元素。但有时候使用css选择器的方式可以更加的方便。使用css选择器的语法，应该使用select 方法。以下列出几种常用的css选择器方法：

#### （1）通过标签名查找：

```python
print(soup.select('a')
```
 ####  (2)通过类名查找：

通过类名，则应该在类的前面加一个 " . "，比如要查找 class=sister 的标签。示例代码如下：

```python
print(soup.select('.sister'))
```
#### （3）通过id查找：

通过id查找，应该在id的名字前面加一个#号。示例代码如下：

```python
print(soup.select("#link1"))
```
#### （4）组合查找：

组合查找即和写class文件时，标签名与类名、id名进行的组合原理是一样的，例如查找p标签中，id等于link1的内容，二者需要用空格分开：

```python
print(soup.select("p #link1"))
```
直接子标签查找，则使用 > 分隔：

```python
print(soup.select("head > title"))
```
#### （5）通过属性查找：

查找时还可以加入属性元素，属性需要用中括号括起来，注意属性和标签属于同一节点，所以中间不能加空格，否则会无法匹配到。示例代码如下：
```python
print(soup.select('a[href='http://example.com/elsie']'))
```
#### （6）获取内容

以上的select方法返回的结果都是列表形式，可以遍历形式输出，让后用get_text()方法来获取它的内容。


**其他请看官方文档**












