# **XPth语法和lxml模块**
## **什么是XPath？**
xpath（XML Path Language）是一门在XML和HTML文档中查看信息的语言，可用来在XML和HTML文档中对元素和属性进行遍历。
## **XPath开发工具**
1、Chrome插件Xpath Helper。
2、Firefox插件Try Xpath。

## **XPath语法**
### **选取节点：**
XPath 使用路径表达式来选取ＸＭＬ文档中的节点或者节点集。这些路径表达式和我们在常规的电脑文件系统中看到的表达式非常相似。

|表达式|描述|示例|结果|
|:----:|:----:|:----:|:----:|
|nodename|选取此节点或者所有子节点|bookstore|选取bookstore下所有的子节点|
|/|如果是在最前面，代表从根点选取。否则选择某个节点下的某个节点|bookstore|选取根元素下所有的bookstore节点|
|//|从全局节点中选择节点，随便在哪个位置|book|从全局节点中找到所有的book节点|
|@|选取某个节点的属性|book[@price]|选择所有拥有price属性的book节点|

谓语：
谓语用来查找某个特定的节点或者包含某个指定的值的节点，被嵌在方括号中。在下面的表格中，我们列出了带有谓语的一些路径表达式，以及表达式的结果：

|路径表达式|描述|
|:----:|:----:|
|/bookstore/book[1]|选取bookstore下的第一个子元素|
|/bookstore/book[last()]|选取bookstore下的倒数第二个book元素。|
|bookstore/book[position()<3]|选取bookstore下前面的两个子元素|
|//book[@price]|选取拥有price属性的book元素|
|//book[@price=10]|选取所有属性price等于10的book元素|


通配符：

__*__ 表示通配符。

|通配符 |描述|示例|结果|
|:----:|:----:|:----:|:----:|
|*|匹配任意节点|/bookstore/*|选取bookstore下的所有子元素|
|@+|匹配节点中的任何属性|//book[@*]|选取所有带有属性的book元素。|

选取多个路径：

通过在路径表达式中使用“|“运算符，可以选取若个路径。
示例代码如下：

```lxml
//div[@class="content_r"] | //dd[@class="job-advantage"]
```



**运算符：**

|\||计算两个节点集|//book \| //cd|返回所有拥有 book 和 cd 元素的节点集|
|:----:|:----:|:----:|:----:|
|+|加法|6 + 4|10|
|-|减法|//book | //cd|2|
|\*|乘法|6\*4|24|
|div|除法|8 div 4|2|
| = |等于|price=9.80|如果 price 等于9.80，返回true，否则返回false|
|！=|不等于|price！=9.80|如果price是不等于9.80，返回ture，否则返回false。|
|<|小于|price<9.80 |如果price小于9.80，返回true，否则返回false|
|<=|小于或等于|price<=9.80|如果price小于或等于9.80，返回true。否则返回false。|
|>|大于|price>9.80|如果price大于9.80，返回true，否则返回false|
|>=|大于或等于|price>=9.80|如果price大于或等于9.80，返回true，否则返回false。|
|or|或|price=9.80 or price=9.90|如果price等于9.80或者price等于9.90，返回ture，否则返回false。|
|and|与|price>9.00 and price<9.90|如果price大于9..80并且price小于9.90，返回true，否则返回flase|
|mod|计算出发的余数|5 mod 2|1|


# lxml库

lxml是一个HTML/XML的解析器，主要的功能是如何解析和提取 HTML/XML 数据。

lxml和正则一样，也是用C 实现的。是一款高性能的 Python HTML/XML 解析器，我们可以利用之前学习的 Xpath语法，来快速的定位特定元素以及节点信息。

lxml python官方文档：http://lxml.de/index.html       

需要安装 C 语言库，可以使用 pip 安装： pip install lxml

## 基本使用

我们可以利用它来解析HTML代码，并且在解析HTML代码的时候，如果HTML代码不规范，它会自动的进行补全。示例代码如下：

```python
#使用lxml 的 etree 库
from lxml import etree

text = '''
<div>
    <ul>
        <li class="item-0"><a href="link1.html">first item</a></li>
        <li class="item-1"><a href="link2.html">second item</a></li>
        <li class="item-inactive"><a href="link3.html">third item</a></li>
        <li class="item-1"><a href="link4.html">fourth item</a></li>
        <li class="item-0"><a href="link5.html">fifth item</a> #注意，此处缺少一个</li>的必合标签
    </ul>
</div>
'''
#利用etree.HTML ，将字符串解析为HTML文档对象
html = etree.HTML(text)

#将HTML文档对象序列化字符串
result = etree.tostring(html)


#返回的是bytes的数据类型
print(result)

#加上encoding('utf-8')，指定用 'utf-8'的编码格式去处理字符串
#result = (etree.tostring(html,encoding="utf-8"))
#加上decode('utf-8'),将bytes数据类型转换为 'utf-8'的数据类型，返回的是utf-8的数据类型
#print(result.decode('utf-8'))
```


输出结果如下：
```python
b'<html><body><div>\n    <ul>\n        <li class="item-0"><a href="link1.html">first item</a></li>\n        <li class="item-1"><a href="link2.html">second item</a></li>\n        <li class="item-inactive"><a href="link3.html">third item</a></li>\n        <li class="item-1"><a href="link4.html">fourth item</a></li>\n        <li class="item-0"><a href="link5.html">fifth item</a>\n    </li></ul>\n</div>\n</body></html>'

```

可以看到，lxml会自动的修改HTML代码。例子中不仅补全了li标签。还添加了body标签，html标签。

从文件中读取html代码：

除了直接使用字符串进行解析， lxml还支持从文件中读取内容，我们新建一个hello.html文件：
```python
from lxml import etree

html = etree.parse('hello.html')
print(etree.tostring(html,encoding="utf-8").decode('utf-8'))
```

hello.html的代码如下：
```python
<div>
    <ul>
        <li class="item-0"><a href="link1.html">first item</a></li>
        <li class="item-1"><a href="link2.html">second item</a></li>
        <li class="item-inactive"><a href="link3.html"><span class="bold">third item</span></a></li>
        <li class="item-1"><a href="link4.html">fourth item</a></li>
        <li class="item-0"><a href="link5.html">fifth item</a></li>
    </ul>
</div>
```
### 使用lxml解析HTML代码：

#### 1、解析html字符串：使用 lxml.etree.HTML 进行解析。示例代码如下：

```python
from lxml import etree

text = '''
<div>
    <ul>
        <li class="item-0"><a href="link1.html">first item</a></li>
        <li class="item-1"><a href="link2.html">second item</a></li>
        <li class="item-inactive"><a href="link3.html"><span class="bold">third item</span></a></li>
        <li class="item-1"><a href="link4.html">fourth item</a></li>
        <li class="item-0"><a href="link5.html">fifth item</a></li>
    </ul>
</div>
'''
html = etree.HTML(text)
result = etree.tostring(html,encoding="utf-8")
print(result.decode('utf-8'))
```
#### 2、解析html文件：使用 lxml.etree.parse 进行解析，示例代码如下：

```python
from lxml import etreehtml = etree.parse('hello.html')print(etree.tostring(html,encoding="utf-8").decode('utf-8'))

```
这个函数默认使用的事 XML 解析器，所有如果碰到一些不规范的 HTML代码的时候就会解析错误，这时候就要自己创建 HTML 解析器。

```python
from lxml import etree

#创建 XML 解析器
parser = etree.HTMLParser(encoding="utf-8")
#使用 XML 解析器
htmlElement=etree.parse("hello.html",parser=parser)
print(etree.tostring(htmlElement,encoding="utf-8").decode('utf-8'))
```
### 在lxml中使用XPath语法：

#### 1、获取所有li标签：

```python
from lxml import etree

html = etree.parse('hello.html')
print(type(html)) #显示etree.parse() 返回类型

result = html.xpath('//li')

print(result) #打印<li>标签的元素集合
```


#### 2、获取所有li元素下的所有class属性的值：

```python
from lxml import etree

html = etree.parse('hello.html')

result = html.xpath('//li/@class')

print(result)
```
#### 3、获取li标签下href 为 www.baidu.com的标签：

```python
from lxml import etree

html = etree.parse('hello.html')

result = html.xpath('//li/a[@href="www.baidu.com"]')
print(result)
```
#### 4、获取li标签下的所有span标签：

```python
from lxml import etree

html = etree.parse('hello.html')

#result = html.xpath('//li/span')
#注意这么些是不对的：
#因为 / 是用来获取子元素的， 而<span> 并不是 <li>的子元素，所以要用双斜杠

result = html.xpath('//li//span')
print(result)
```

#### 5、获取li标签下载a标签里的所有class：

```python
from lxml import etree

html = etree.parse('hello.html')

result = html.xpath('//li/a/@class')
print(result)
```
### lxml结合xpath注意事项：

#### 1、使用 xpath 语法，应该使用 Element.xpath 方法。来执行xpath的选择。示例代码如下：

```python
trs = html.xpath("//tr[position()>1]"
#获取第二行开始的所有tr标签
```
xpath函数返回的永远是一个列表。

#### 2、获取某个标签的属性

```python
href = html.xpath("//a/@href")
#获取所有a标签的href属性的值
```

#### 3、获取文本，是通过 xpath中的 text() 函数，示例代码如下：

```python
address = tr.xath(".'td[4]/text()")[0]
#获取返回列表的第一个元素
```
#### 4、在某个标签下，再执行xpath函数，获取这个标签下的子元素，那么应该在//之前加一个点，代表的是当前元素下获取。示例代码如下：

```python
address = tr.xath(".'td[4]/text()")[0]
#获取返回列表的第一个元素

```






