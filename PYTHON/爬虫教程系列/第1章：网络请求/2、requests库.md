# requests库

虽然Python的标准库中 urllib模块已经包含了我们平常使用的大多数功能，但是它的API使用起来让人感觉不太好，而Requests宣传是 “HTTP for Huamans”，说明使用更简洁方便。

安装和文档地址：

利用 pip 可以非常方便的安装：

```pip
pip install requests
```

中文文档： http://docs.python-requests.org/zh_CN/latest/index.htmlgithub地址：https://github.com/requests/requests

## 发送GET请求
## 1、最简单的发送get请求就是通过 request.get 来调用：
```python
resonse = requests.get("http://www.baidu.com/")
```

## 2.添加headers和查询参数：
```python
import requests
kw = {'wd':'中国'}
headers = {"User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:68.0) Gecko/20100101 Firefox/68.0"}
#params 接受一个字典或者字符串的查询参数，字典类型自动转换为url编码，不需要urlencode()
response = requests.get("http://www.baidu.com/s", params = kw, headers = headers)
#查看响应内容, response.text 返回的是Unicode格式的数据
print(respsonse.text)
#查看响应内容, response.content 返回的是字节流的数据   
print(respsonse.content)
# 查看完整url地址
print(response.url)
#查看响应头字符编码
print(response.encoding)
#查看响应码
print(response.status_code)
```


## 发送POST请求：
### 1、最基本的POST请求可以使用 post 方法：
```python
respon = requests.post("http://www.baidu.com/",data=data)
```

### 2、传入 data数据：这时候就不要再使用 urlencode进行解码了，直接传入一个字典进去就可以了。比如请求拉勾网的数据的代码：
```python
url = 'https://www.lagou.com/jobs/positionAjax.json?needAddtionalResult=false'

headers = {  "Referer": "https://www.lagou.com/jobs/lis…rds=&fromSearch=true&suginput=",  "User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:68.0) Gecko/20100101 Firefox/68.0"}
data = {  'first':"true",  'pn':1,  'kd':"python"}

response = requests.post(url,data=data,headers=headers)

#如果是json数据，直接可以调用json方法print(response.text)
```
## 使用代理：
 使用requests 添加代理也非常简单，只要在请求的方法中（比如 get 或者 post） 传递 proxies 参数就可以了。示例代码如下：
```python
import requests
#没有使用代理
# response = requests.get("http://httpbin.org/ip")# print(response.text)

#使用代理的proxy = {  'http':"117.80.234.163:808"}response = requests.get("http://httpbin.org/ip",proxies=proxy)
print(response.text)
```

## cookie：

如果在一个响应中包含了 cookie， 那么可以利用 cookies 属性拿到这个返回的 cookie 值：
```python
import requests
response = requests.get('https://www.baidu.com/')#打印cookie信息
print(response.cookies)
#将cookie信息以字典的形式打印出来
print(response.cookies.get_dict())
```
## session：
之前使用 urllib库，是可以使用opener发送多个请求，多个请求之间可以共享 cookie的。那么如果使用 requests，也要达到共享 cookie的目的，那么可以使用requests 库 给我们提供的 session对象。注意，这里的seesion 不是web 开发中的session，这个地方只是那么一个的对象而已。还是以登陆人人网为例，使用requests来实现。示例代码如下：

```python
import requests
login_url = 'http://www.renren.com/PLogin.do'
data = {    'email':"13539439659",    'password':"2019HUANG520.."  }
headers = {    'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:68.0) Gecko/20100101 Firefox/68.0'  }
#创建一个session对象
session = requests.Session()
#用session对象发送请求
session.post(login_url,data=data,headers=headers)
response = session.get('http://www.renren.com/872201594/profile')
#将返回内容写入到文件中
with open('renren.html','w',encoding='utf-8') as fp:  fp.write(response.text)
```
## 处理不信任的SSL证书：

对于那些已经被信任的SSL证书的网站，比如 https://www.baidu.com/s ,那么使用requests 直接添加verify属性就可以正常的返回响应。示例代码如下：
```python
import requests

#添加verify属性就可以忽略检测SSL证书
response = requests.get("http://www.12306.cn/",verify =False)print(response.content.decode('utf-8'))
```

## 【实战】爬虫拉勾网职位信息
```python
import requests, time
from lxml import etree

def request_list_page():  
    url_start = 'https://www.lagou.com/jobs/list_python?px=default&city=%E5%8C%97%E4%BA%AC'  
    url_parse = 'https://www.lagou.com/jobs/positionAjax.json?px=defaulcity=%E5%8C%97%E4%BA%AC&needAddtionalResult=false'  
    headers = {    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64)AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.100Safari/537.36",    "Referer": "https://www.lagou.com/jobs/list_python?px=default&city=%E5%8C%97%E4%BA%AC"  }      data = {    'first': 'true',    'pn': '1',    'kd': 'python'  } 
    # 创建一个session对象  
    session = requests.Session()  # 用session对象发送请求 
    session.get(url_start, headers=headers,timeout=3) 
    cookie = session.cookies    
    response = session.post(url_parse, data=data, headers=headers)# json方法，如果返回来的是json数据，那么这个方法会自动的load成字典    
    print(response.json())
request_list_page()
```

