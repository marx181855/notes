# urllib库

urllib库是Python是中一个最基本的网络请求库。可以模拟浏览器的行为,向指定的服务器发送一个请求,并可以保存服务器返回的数据。

## urlopen函数:

在 Python3的urllib库中,所有和网请求相关的方法,都被集到request模块下面了,先来看下 urlopen函数基本的使用:

```python
from urllib import request
resp = request. urlopen('http://www.baidu.com')
print(resp. read())
```
实际上,使用浏览器访问百度,右键查看源代码。你会发现,跟我们刚才打印出来的数据是一模一样的。也就是说,上面的三行代码就已经帮我们把百度的首页的全部代码爬下来了。一个基本的ur请求对 python应的代码真的非常简单。以下对 urlopen函数的进行详细讲解:

1、url：请求的url。
2、data：请求的data,如果设置了这个值,那么将变成pos请求。
3.返回值:返回值是一个http.client. HTTPResponse对象，这个对象是一个类文件句柄对象。
有read(size)、readline、readlines 以及 getcode等方法 。

## urlretrieve函数:

这个函数可以方便的将网页上的一个文件保存到本地。一下到吗可以非常方便的将百度的首页下载到本地：

```python
from urllib import request
request.urlretrieve('http://www.baidu.com','baidu.html')
```

## urlencode函数

用浏览器发送请求的时候，如果url包含了中文或者其他特殊字符，那么浏览器会自动的给我们进行编码。而如果使用代码发送请求，那么就必须手动的进行编码，这时候就应该使用 urlencode 函数实现。urlencode函数来实现。 urlencode 可以把字典数据转换为 URL 编码的数据。
示例代码如下：

```python
from urllib import parse
data = {'name':'爬虫基础','greet':'hello world','age':100}
qs = parse.urlencode(data)
print(qs)
```
## parse_qs函数

可以将进行编码过的url参数进行解码。示例代码如下：
```python
from urllib import parse
qs = 'name=%E7%88%AC%E8%99%AB%E5%9F%BA%E7%A1%80&greet=hello+world&age=100'
print(parse.parse_qs(qs))
```

## urlparse和urlsplit

有时候拿到一个url，想要对这个url中的各个组成部分进行分割，那么这时候就使用 urlparse或者urlsplit来进行分割。示例代码如下：

```python
from urllib import request,parse
url = 'http://www.baidu.com/s?username=zhiliao'
#result = parse.urlparse(url)
result = parse.urlsplit(url)
print('scheme:',result.scheme)
print('netloc:',result.netloc)
print('path:',result.path)
print('query:',result.query)
```

urlparse 和 urlsplit 基本上是一模一样的。唯一不一样的地方是， urlparse 里面多了一个 params 属性，而urlsplit 没有这个 params属性。比如有一个 url 为：url = 'http://www.baidu.com/s;hello?wd=python&username=abc#1' ,那么urlparse可以获取到hello，而urlsplit 不可以获取到。url中的params也用得比较少。

## request.Request类

如果想要在请求的时候增加一些请求头，那么就必须使用 reques.Request 类来实现。比如要增加一个User-Agent，示例代码如下：

```python
from urllib import request
headers = {
    'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:68.0) Gecko/20100101 Firefox/68.0'
}
req = request.Request("http://www.baidu.com/",headers=headers)
resp = request.urlopen(req)
print(resp.read())
```

## ProxyHandler处理器（代理设置）

很多网站会检测某一段时间某一个IP的访问次数（通过流量统计，系统日志等），若果访问次数的不像正常人，它就会禁止这个IP的访问。所以我们可以设置一些代理服务器，每隔一段时间换一个代理，就算IP被禁止，依然可以换个IP继续爬取。
urllib中通过ProxyHandler来设置使用代理服务器：
1、代理的原理：在请求目的网站之前，先请求代理服务器，然后让代理服务器去请求目的网站， 代理服务器拿到目的网站的数据后，再转发给我们的代码。
2、http://200019.ip138.com：这个网站可以方便查看IP地址。
3、在代码中使用代理：
	* 
使用 'urllib.request.ProxyHandler' , 传入一个代理， 这个代理是一个字典，字典的key依赖于代理服务器能够接受的类型， 一般是 'http' 或者 'https', 值是’IP、：port‘。
	* 
使用上一步创建的‘handler’，以及‘request.build_opener’创建一个' opener' 对象。
	* 
使用上一步的 ‘opener’，调用‘open ’函数，发起请求。


下面代码说明如何使用自定义opener来使用代理：
```python
from urllib import request,parse
#没有使用代理的
# url =' http://200019.ip138.com/'
# resp = request.urlopen(url)
# print(resp.read().decode('utf-8'))


#使用代理的
url = 'http://200019.ip138.com/'
# 1. 使用ProxyHandler，传入代理构建一个handle
handler = request.ProxyHandler({"http":"106.12.116.128:808"})
# 2. 使用上面创建的hanler构建一个opener
opener = request.build_opener(handler)
# 3. 使用openeru发送一个请求
resp = opener.open(url)
print(resp.read().decode('utf-8'))
```


## 什么是cookie：

在网站中，http请求是无状态的。也就是说即使第一次和服务器连接后并且登录成功后，第二次请求服务器依然不能知道当前请求是哪个用户。 cookie的出现就是为了解决这个问题，第一次登陆后服务器返回一些数据（cookie）给浏览器，然后浏览器保存在本地，当该用户发送第二次请求的时候，就会自动把上次请求储存的cookie 数据自动的携带给服务器，服务器通过浏览器携带的数据就能判断当前用户是哪个了。 cookie储存的数据量有限，不同的浏览器有不同的储存大小，但一般不超过4KB。因此 cookie 只能储存一些小量的数据。

cookie的格式：

Set-Cookies: NAME=VALUE: Expires/Max-age=DATE: Path=PATH:Domain=DOMAIN_NAME: SECURE

参数意义： 

- NAME： cookie的名字。  
- VALUE：cookie的值。  
- Expires：cookie的过期时间。  
- Path：cookie作用的路径。  

Domain：cookie作用的域名。SECURE：是否只在https协议下起作用。



## 使用cookielib库和HTTPCookieProcessor模拟登陆：

Cookie是指网站服务器为了辨别用户身份和进行Session跟踪，而储存在用户浏览器上的文本文件， Cookie可以保持登陆信息刀用户下次与服务器的会话。
这里以人人网为例。人人网中，要访问某个人的主页，必须先登录才能访问，登陆说白了就是要有cookie信息。那么如果我们想要用代码的方式访问，就必须要有正确的cookie信息才能访问。解决方案有两种，第一种是使用浏览器访问，然后将cookie信息复制下来，放到header中。示例代码如下：

```python
#别人主页 ：http://www.renren.com/872201594/profile
from urllib import request


#1：不使用coolie去请求别人主页

url = "http://www.renren.com/872201594/profile"
# resp = request.urlopen(url)
# with open('renren.html','w',encoding='utf-8') as fp:
#     fp.write(resp.read().decode('utf-8'))


#2： 使用cookie去请求别人主页
headers = {
    'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:68.0) Gecko/20100101 Firefox/68.0',
    "Cookie":"anonymid=jywcpwftfy0usg; depovince=GW; jebecookies=340bfc4f-ce60-4ce6-819f-044ccd565a5f|||||; _r01_=1; JSESSIONID=abcNgdhLb0C5uatXvEAXw; ick_login=4f826a0a-d839-4890-9c20-80c1239b20e8; t=b5a9c7b6dab12c2e2ef78f2844d6b45b1; societyguester=b5a9c7b6dab12c2e2ef78f2844d6b45b1; id=971767161; xnsid=a575ecc4; ver=7.0; loginfrom=null; jebe_key=dc383e46-3e6e-4caa-b4fc-8be92cb9fff2%7Cac0d4178c34b954854edf4e416dff0c8%7C1564887095678%7C1%7C1564887094536; wp_fold=0"
}
requ = request.Request(url,headers = headers)
resp = request.urlopen(requ)
with open('renren.html','w',encoding='utf-8') as fp:
     fp.write(resp.read().decode('utf-8'))

```

当时每次在访问需要cookie的页面都要从浏览器中复制coolie比较麻烦。在Python处理Cookie，一般是通过http.cookiejar 模块和urllib模块和HTTPCoolieProcessor处理器类一起使用。 http.cookiejar 模块主要作用是提供用于存储cookie的对象。
而 HTTPCookieProcessor 处理器主要作用是处理这些cookie对象，并构建handler对象。

## http.cookiar模块：

该模块主要的类有CookieJar、FileCookieJar、MozillaCookieJar、LWPCookieJar。这四个类的作用分别如下：

1. CookieJar : 管理HTTP  cookie的值、存储HTTP请求生成的cookie、向传出的HTTP请求添加cookie的对象。整个cookie都存储在内存中,对 CookieJarcookie例进行垃圾回收后也将丢失。

2. FileCookieJar（filename ，delayload=None，policyNone:从CookieJar派生而来,用来创建 FileCookieJar实例，检索 cookie信息并将cookie存储到文件中。 filename是存储cookie的文件名 delayload为True时支持延迟访问访问文件,即只有在需要时才读取文件或在文件中存储数据。

3. MozillaCookieJar(filename,delayload=None,policyNone):从 FileCookieJar派生而来,创建与 Mozillacookie浏览器cookie.txt兼容的 FileCookieJar实例

4. LWPCookieJar(filename，delayload=None，policy=None):从 FileCookieJar派生而来,创建与 libwww--per准的set- Cookie3文件格式兼容的 FileCookieJar实例。

## 利用 Http. cookiejar 和 request.HTTPCookieProcessor 登录人人网。相关示例代码如下:

```python
#别人主页：http://www.renren.com/872201594/profile
#人人网登陆url：http://www.renren.com/SysHome.do

from urllib import request
from urllib import parse
from http.cookiejar import CookieJar

# 1. 登陆
# 1.1 创建一个cookiejar对象
cookiejar = CookieJar()
# 1.2 使用cookiejar创建一个HTTPCookieProcess对象
handler = request.HTTPCookieProcessor(cookiejar)
# 1.3 使用上一步创建的handler创建一个opener
opener = request.build_opener(handler)
# 1.4 使用opener发送登陆的请求（人人网的邮箱和密码）
headers = {
    'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:68.0) Gecko/20100101 Firefox/68.0'
}
data = {
    'email':"13539439659",
    'password':"2019HUANG520.."
}
login_url = 'http://www.renren.com/PLogin.do'
req = request.Request(login_url,data = parse.urlencode(data).encode('utf-8'),headers=headers)
opener.open(req)


#2 . 访问个人主页
url = 'http://www.renren.com/872201594/profile'
#获取个人的页面的时候，不要新建一个opener
#而应该使用之前的那个opener，因为之前的那个opener已经包含了登陆所需要的cookie信息
req = request.Request(url,headers=headers)
resp = opener.open(req)
with open('renren.html','w',encoding='utf-8') as fp:
    fp.write(resp.read().decode('utf-8'))


优化后的代码：

#别人主页：http://www.renren.com/872201594/profile
#人人网登陆url：http://www.renren.com/SysHome.do

from urllib import request
from urllib import parse
from http.cookiejar import CookieJar

headers = {
        'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:68.0) Gecko/20100101 Firefox/68.0'
    }

def get_opener():
    # 1. 登陆
    # 1.1 创建一个cookiejar对象
    cookiejar = CookieJar()
    # 1.2 使用cookiejar创建一个HTTPCookieProcess对象
    handler = request.HTTPCookieProcessor(cookiejar)
    # 1.3 使用上一步创建的handler创建一个opener
    opener = request.build_opener(handler)
    # 1.4 使用opener发送登陆的请求（人人网的邮箱和密码）
    return opener


def login_renren(opener):
    data = {
        'email':"13539439659",
        'password':"2019HUANG520.."
    }
    login_url = 'http://www.renren.com/PLogin.do'
    req = request.Request(login_url,data = parse.urlencode(data).encode('utf-8'),headers=headers)
    opener.open(req)


def visit_profile(opener):
    #2 . 访问个人主页
    url = 'http://www.renren.com/872201594/profile'
    #获取个人的页面的时候，不要新建一个opener
    #而应该使用之前的那个opener，因为之前的那个opener已经包含了登陆所需要的cookie信息
    req = request.Request(url,headers=headers)
    resp = opener.open(req)
    with open('renren.html','w',encoding='utf-8') as fp:
        fp.write(resp.read().decode('utf-8'))


if __name__ == '__main__':
    opener = get_opener()
    login_renren(opener)
    visit_profile(opener)
```

## 保存cookie到本地：

保存cookie到本地，可以使用cookiejar的save方法，并且需要指定一个文件名：

```python
from urllib import request
from http.cookiejar import MozillaCookieJar
#创建一个cookiejar对象
cookiejar = MozillaCookieJar('cookie.txt') #这里输入了文件名，下面的cookiejar.save()就不用再输入文件名
cookiejar.load(ignore_discard=True)#加载本地cookie信息，cookiejar对象输入了文件名，这里就不用输入
#使用cookiejar创建一个HTTPCookieProcess对象
handler = request.HTTPCookieProcessor(cookiejar)
#使用上一步创建的handler创建一个opener
opener = request.build_opener(handler)


resp = opener.open('http://httpbin.org/cookies/set?course=abc')
for cookie in cookiejar:
    print(cookie)
#保存cookie
#cookiejar.save(ignore_discard=True)
#ignore_discard 保存即将过期的cookie信息


```