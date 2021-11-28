# 一、准备

安装模块`pip install markdownify`

# 二、基本使用

```python
from markdownify import markdownify

html = markdownify("<h1>Hello, World!</h1>")

print(html)

'''
Hello World
===========
'''
```

默认输出的是markdown的Setext标题语法

# 三、下面是输出markdown的ATX标题语法实例

```python
from markdownify import markdownify

html = markdownify("<h1>Hello, World!</h1>", heading_style="ATX")

print(html)

'''
## Hello, World!
'''
```

# 四、转载小程序

```python
import requests
from lxml import etree
from markdownify import markdownify

def reprintBlog(url,selector):
    headers = {
        'user-agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/89.0.4389.128 Safari/537.36 Edg/89.0.774.77'
    }
    htmlTextStr = requests.get(url,headers=headers).text
    with open('text.txt','w',encoding='utf-8') as f:
        f.write(htmlTextStr)
    html = etree.HTML(htmlTextStr)
    blogContent = html.xpath(selector)
    blogHtmlContent = etree.tostring(blogContent[0],encoding="utf-8").decode('utf-8')
    html = markdownify(blogHtmlContent, heading_style="ATX")

    print(html)

```



------

参考：https://brianli.com/python-convert-html-markdown/