[TOC]



# 动态网页抓取

# 什么是AJAX：

AJAX：（  Asynchronous Javascript And XML）异步javascript和XML。过在后台与服务器进行少量数据交换，Ajax可以使网页实现异步更新。这意味着可以异步重载网页页面。因为传统的传输数据格式方面使用的是XML语法。因此叫做 AJax，其实现在数据交互基本上都是使用 JSON。使用 AJax加载的数据，即使是用了JS，将数据渲染到了浏览器中，在右键->查看源代码 还是不能通过ajax加载的数据，只能看到使用这个url加载的html代码。

## 获取ajax数据的方式：

1、直接分析ajax调用的借口。然后通过代码请求这个借口。
2、使用Selenium+chromedriver模拟浏览器行为进行获取数据。

|   方式   |                             优点                             |                             缺点                             |
| :------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
| 分析借口 |  直接可以请求到数据。不需要做一些解析工作。代码量少，性能高  | 分析接口比较复杂，特别是一些通过js混淆的接口，要有一定的js功底。容易被发现时爬虫。 |
| selenium | 直接模拟浏览器的行为。浏览器能请求到的，使用selenium也能请求到。爬虫更稳定 |                       代码量多。性能低                       |

## **Selenium+chromedriver获取动态数据：**

Selenium相当于是一个机器人。可以模仿人类在浏览器上的一些行为，自动处理浏览器上的一些行为，比如点击，填充数据，删除cookie等。chromedriver是一个驱动 CHrome浏览器的驱动程序，使用它才可以驱动浏览器。当然针对不同的浏览器有不同的driver。以下列出了不同浏览器及其对应的driver：
1、Chrome：http://chromedriver.storage.googleapis.com/index.html
2、Firefox：https://github.com/mozilla/geckodriver/releases
3、EDge：https://developer.microsoft.com/en-us/microsoft-edge/tools/webdriver/
4、Safari：https://wbkit.org/blog/6900/webdriver-support-in-safari-10/

## **安装Selenium和chromedriver**

1、安装 Selenium：Selenium有很多语言的版本，有java、ruby、python等，我们下载python版本的就行了。
```
pip install selenium
```
2、安装 chromedirver：下载完成后，放到不需要权限的纯英文目录下就可以了。

**快速入门：**

现在以一个简单的获取百度首页的例子来讲下 Selenium 和 chromedriver 如何快速入门：

## selenium常用操作：

更多教程请参考：http://selenium-python.readthedocs.io/installation.html#introduction

### 关闭页面：

1、 driver.close( ) ：关闭当前页面。
2、 driver.quit( )：推出整个浏览器。

### 定位元素：

1、find_element_by_id：根据id来查找某个元素。等价于：

```PYTHON
submitTag = driver.find_element_by_id('su')
submitTag = driver.find_element(By.ID,'su')
````
2、 find_element_by_class_name：根据类名查找元素。等价于：

```python
submitTag = driver.find_element_by_class_name('su')
submitTag = driver.find_element(By.CLASS_NAME,'su')
```

3、 find_element_by_name：  根据name属性的值来查找元素。等价于：

```python
submitTag = driver.find_element_by_name('su')
submitTag = driver.find_element(By.NAME,'su')
```
4、 find_element_by_tag_name：根据标签名查找元素。等价于：

```python
submitTag = driver.find_element_by_tag_name('div')
submitTag = driver.find_element(By.TAG_NAME,'div')
```
5、 find_element_by_xpath：根据xpath语法来获取元素。 等价于：

```python
submitTag = driver.find_element_by_xpath('//div')
submitTag = driver.find_element(By.XPATH,'//div')
```
要注意，find_element 是获取第一个满足条件的元素。find_elements 是获取所有满足条件的元素。

```python
from selenium import webdriver

# chromedriver的绝对路径
driver_path =
r"D:\wampserver64\www\PYTHON\driver\chromedriver.
exe"
# 初始化一个driver，并且指定chromedriver的路径
driver = webdriver.Chrome
(executable_path=driver_path)
# 请求网页
# 通过page_source获取网页源代码
driver.get("https://www.baidu.com/")

# inputTag = driver.find_element_by_id('kw')

# inputTag = driver.find_element_by_name('wd')

# inputTag = driver.find_element_by_class_name
('s_ipt')

# inputTag = driver.find_element_by_css_selector(".quickdelete-wrap > input")

inputTag = driver.find_elements_by_css_selector(".quickdelete-wrap > input")[0]

inputTag.send_keys('python')


```
### 操作表单元素：

1、操作输入框：分为两步。第一步：找到这个元素。第二部：使用 send_keys(value)，将数据填充进去。示例代码如下：

```python
inputTag = driver.find_element_by_id('kw')
inputTag.send_keys('python')
```
使用clear 方法可以清楚输入框中的内容。示例代码如下：

```python
inputTag.clear()
```
2、操作 checkbox 因为要选中 checkbox标签，在网页中是通过鼠标点击的。因此想要选中 checkbox标签，那么先选中这个标签，然后执行 click 事件。示例代码如下：

```python
rememberTag = driver.find_element_by_name("rememberMe")
rememberTag.click()
```
3、选择 select：select元素不能直接点击。因为点击后还需要选中元素。这时候selenium就专门为select便签提供了一个类 selenium.webdriver.support.ui.Select 。 将获取到的元素当成参数传到这个类中，创建这个对象。以后就可以使用这个对象进行选择了。示例代码如下：

```python
from selenium.webdriver.support.ui import Select
from selenium import webdriver

driver_path = r"D:\wampserver64\www\PYTHON\driver\chromedriver.ex
# 初始化一个driver，并且指定chromedriver的路径
driver = webdriver.Chrome(executable_path=driver_path)
# 请求网页
driver.get("https://www.w3school.com.cn/tiy/t.asp?f=html_select")
#选中这个标签，然后使用Select创建对象
selectTag = Select(driver.find_element_by_name('jumpMenu'))
# 根据索引选择
selectTag.select_by_index(1)
#根据值选择
selectTag.select_by_value("http://www.95yueba.com")
#根据可视的文本选择
selectTag.select_by_visible_text("95秀客户端")
#取消选中的所有项
selectTag.deselect_all()
```
4、操作按钮：操作按钮有很多种方式。比如单机、右击、双击等。这里讲一个最常用的。就是点击，直接调用 click 函数就可以了。示例代码如下：

```python
rememberTag = driver.find_element_by_name("rememberMe")
rememberTag.click()

```
### 鼠标行为链：

有时候在页面中的操作可能有很多不，那么这时候可以使用鼠标行为链 ActionChains 来完成。比如现在要将鼠标移动到某个元素上并执行点击事件。那么示例代码如下：

```python
from selenium import webdriver
from selenium.webdriver.common.action_chains import ActionChains

# chromedriver的绝对路径
driver_path =
r"D:\wampserver64\www\PYTHON\driver\chromedriver.exe"
# 初始化一个driver，并且指定chromedriver的路径

driver = webdriver.Chrome(executable_path=driver_path)
# 请求网页

driver.get("https://www.baidu.com/")
inputTag = driver.find_element_by_id('kw')
submitTag = driver.find_element_by_id('su')
actions = ActionChains(driver)
actions.move_to_element(inputTag)
actions.send_keys_to_element(inputTag,'hhh')
actions.click(submitTag)
actions.perform()
```
还有更多的鼠标相关的操作。

- click_and_hod(element)：点击但不松开鼠标。 
- context_click(element)：右键点击。 
- double_click(element)：双击
更多方法请参考： http://selenium-python.readthedocs.io/api.html


### Cookie操作：

1、获取所有的cookie：

```python
for cookie in driver.get_cookies():
    print(cookie)
```
2、根据cookie的key获取 vlaue：

```python
value = driver.get_cookie(key)
```
3、删除所有的cookie：

```python
driver.delete_all_cookies()
```
4、删除某个 cookies：

```python
driver.delete_cookie(key)
```
```python
from selenium import webdriver


# chromedriver的绝对路径
driver_path = r"D:\wampserver64\www\PYTHON\driver\chromedriver.exe"

# 初始化一个driver，并且指定chromedriver的路径
driver = webdriver.Chrome(executable_path=driver_path)

# 请求网页
driver.get("https://www.baidu.com/")

# 获取所有的cookie信息
# for cookie in driver.get_cookies():

#     print(cookie)

# 获取某个cookie信息
# value = driver.get_cookie('H_PS_PSSID')
# print(value)

# 删除某个cookie信息
# driver.delete_cookie('H_PS_PSSID')

# 删除所有的cookie信息
driver.delete_all_cookies()

```

### 页面等待：

现在的网页越来越多采用了Ajax 技术，这样程序便不能确定何时某个元素完全加载出来了。如果实际页面等待时间过长导致某个dom元素还没出来，但是你的代码直接使用了 WebElement，那么就会抛出 NulllPointer的异常。为了解决这个问题。所有　Selenium提供了两种等待方式：一种是隐式等待、一种是显式等待。

1、隐式等待：调用 driver.implicity_wait。那么在获取任何元素之前，会先等待10秒钟的时间。示例代码如下：

```python
from selenium import webdriver

# chromedriver的绝对路径
driver_path = r"D:\wampserver64\www\PYTHON\driver\chromedriver.exe"

# 初始化一个driver，并且指定chromedriver的路径
driver = webdriver.Chrome(executable_path=driver_path)

# 请求网页
driver.get("https://www.douban.com/")

driver.implicitly_wait(20)

driver.find_element_by_id('sdfdsfsdfsd')
​```python
2、显示等待：现实等待是表明某个条件成立后才执行获取元素的操纵。也可以在等待的时间指定一个量大的时间，如果超过这个时间那么就抛出一个异常。现实等待应该使用 selenium.webdriver.support.excepted_conditions 期望的条件和 selenium.webdriver.support.ui.WebDriverWait来配合完成。示例代码如下：

​```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

# chromedriver的绝对路径
driver_path = r"D:\wampserver64\www\PYTHON\driver\chromedriver.exe"

# 初始化一个driver，并且指定chromedriver的路径
driver = webdriver.Chrome(executable_path=driver_path)

# 请求网页
driver.get("https://accounts.douban.com/passport/login_popup?
login_source=anony")

element = WebDriverWait(driver, 10).until(
    EC.presence_of_element_located((By.ID, 'code'))
)

print(element)
```
3、一些其他的等待条件： 

 - presence_of_element_located：某个元素已经加载完毕了 
 - presence_of_all_element_located：网页中所有的元素都加载完毕了.
 - element_to_be_cliable：某个元素是可以点击了。
更多条件请参考：https://selenium-python.readthedocs.io/waits.html

**隐式等待和显示等待的区别：**
隐式等待设置一个等待时间，等待时间到了以后才查找元素，如果元素不存在则抛出一个异常。

显示等待设置一个等待时间，在时间内，如果找到元素，停止等待，如果在等待时间内没有找到元素，则到等待时间到了以后抛出一个异常。



### 切换页面：

有时候窗口中有很多子窗口，这时候页面肯定是需要切换的。 selenium 提供了一个叫做 switch_to_window 来进行切换，具体切换到哪个页面，可以从 driver.window_handles 中找到。示例代码如下：

```python
from selenium import webdriver

# chromedriver的绝对路径
driver_path = r"D:\wampserver64\www\PYTHON\driver\chromedriver.exe"

# 初始化一个driver，并且指定chromedriver的路径
driver = webdriver.Chrome(executable_path=driver_path)

# 请求网页
driver.get("https://www.baidu.com/")

# 打开一个新的页面,driver.execute_script()里面可以执行js代码
driver.execute_script("window.open('https://www.douban.com/')")

#打印窗口句柄
print(driver.window_handles)

# # 切换到这个新的页面中
driver.switch_to_window(driver.window_handles[1])

print(driver.current_url)
```
注意：
	1. 虽然在窗口中切换到了新的页面，但是driver中还没有切换。
	2. 如果想要在代码中切换到新的窗口，并且做一些爬虫
	3. 那么应该使用driver.switch_to_window 来切换到指定的窗口
	4. 从 driver.window_handles 中取出第几个窗口
	5. driver.window_handles 是一个列表，里面装的是窗口句柄
	6. 它会按照打开的顺序来储存窗口的句柄



### 设置代理ip：

有时候频繁爬取一些网页。服务器发现你是爬虫后会封掉你的ip地址。这时候我们就可以更改代理ip。更改代理ip，不同的浏览器有不同的实现方式。这里是 Chrome 浏览器为例来讲解：

```python
from selenium import webdriver

options = webdriver.ChromeOptions()
options.add_argument("--proxy-server=http://112.109.198.106:3128")

# chromedriver的绝对路径
driver_path = r"D:\wampserver64\www\PYTHON\driver\chromedriver.exe"

# 初始化一个driver，并且指定chromedriver的路径
driver = webdriver.Chrome(executable_path=driver_path,chrome_options=options)

# 请求网页
driver.get("http://httpbin.org/ip")
```
### WebElement元素：

from selenium.webdriver.remote.webelement import  WebElement 类是每个获取出来的元素的所属类。
有一些常用的属性：

1、 get_attrbute：这个标签的某个属性的值。
2、 screentshot：获取当前页面的图解。这个方法只能在driver上使用。
driver 的对象类，也是继承自 WebElement.
更多请阅读相关源代码。

## 【实战】使用Selenium实现拉勾网爬虫的2种代码对比

### 传统方式：



```python
import requests, time, re
from lxml import etree



headers = {
"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.100 Safari/537.36",
"Referer": "https://www.lagou.com/jobs/list_python?px=default&city=%E5%8C%97%E4%BA%AC",
"Host": "www.lagou.com"
}


def request_list_page(headers):
url_start = 'https://www.lagou.com/jobs/list_python?px=default&city=%E5%8C%97%E4%BA%AC'
url_parse = 'https://www.lagou.com/jobs/positionAjax.json?px=default&city=%E5%8C%97%E4%BA%AC&needAddtionalResult=false'


for x in range(1, 5):
data = {
'first': 'true',
'pn': str(x),
'kd': 'python'
}



# 创建一个session对象
session = requests.Session()
# 用session对象发送请求
session.get(url_start, headers=headers,timeout=3)
cookie = session.cookies
response = session.post(url_parse, data=data, headers=headers,cookies=cookie)



# json方法，如果返回来的是json数据，那么这个方法会自动的load成字典
result = response.json()
# print(result)
positions = result["content"]["positionResult"]["result"]
for position in positions:
positionId = position['positionId']
position_url = 'https://www.lagou.com/jobs/%s.html'% positionId
# print(position_url)
parse_position_detail(position_url,headers,cookie)
time.sleep(3)
break



def parse_position_detail(url,headers,cookies):
response = requests.get(url=url,headers=headers,cookies=cookies)
text = response.content.decode('utf-8')
html = etree.HTML(text)
postion_name = html.xpath("//h2[@class='name']/text()")
job_request_infos = html.xpath("//dd[@class='job_request']//span/text()")
salary = re.sub(r"[\s/]","",job_request_infos[0])
city = re.sub(r"[\s/]","",job_request_infos[1])
work_years = re.sub(r"[\s/]","",job_request_infos[2])
education = re.sub(r"[\s/]","",job_request_infos[3])
job_detail = "".join(html.xpath("//dd[@class='job_bt']//text()")).strip()
print(job_detail)

request_list_page(headers)
```



### 使用selenium来爬取拉勾网职位详细信息



```python
# 使用selenium来爬取拉勾网职位详细信息
from selenium import webdriver
from lxml import etree
import re
import time
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.common.by import By
class LagouSpider(object):
    driver_path = r"D:\wampserver64\www\PYTHON\driver\chromedriver.exe"
    def __init__(self):
        self.driver = webdriver.Chrome(executable_path=self.driver_path)
        self.url = 'https://www.lagou.com/jobs/list_python?px=default&city=%E5%8C%97%E4%BA%AC'
        self.positions = []
    def run(self):
        self.driver.get(self.url)
        while True:
            source = self.driver.page_source
            WebDriverWait(driver=self.driver,timeout=10).until(
                EC.presence_of_element_located((By.XPATH,"//div[@class='pager_container']/span[last()]"))
            )
            self.parse_list_page(source)
            next_page_btn = self.driver.find_element_by_xpath(
                "//div[@class='pager_container']/span[last()]")
            if "page_next_disable" in next_page_btn.get_attribute("class"):
                pass
            else:
                next_page_btn.click()
            time.sleep(1)
    def parse_list_page(self, source):
        html = etree.HTML(source)
        links = html.xpath("//a[@class='position_link']/@href")
        for link in links:
            self.request_detail_page(link)
            time.sleep(1)
    def request_detail_page(self, url):
        #self.driver.get(url)
        self.driver.execute_script("window.open('%s')" %url)
        self.driver.switch_to.window(self.driver.window_handles[1])
        WebDriverWait(driver=self.driver,timeout=10).until(
                EC.presence_of_element_located((By.XPATH,"//h2[@class='name']"))
             )
        source = self.driver.page_source
        self.parse_detail_page(source)
        # 关闭当前这个窗口
        self.driver.close()
        # 继续切换回职业列表
        self.driver.switch_to.window(self.driver.window_handles[0])
    def parse_detail_page(self, source):
        html = etree.HTML(source)
        postion_name = html.xpath("//h2[@class='name']/text()")
        job_request_infos = html.xpath(
            "//dd[@class='job_request']//span/text()")
        salary = re.sub(r"[\s/]", "", job_request_infos[0])
        city = re.sub(r"[\s/]", "", job_request_infos[1])
        work_years = re.sub(r"[\s/]", "", job_request_infos[2])
        education = re.sub(r"[\s/]", "", job_request_infos[3])
        company_name = html.xpath("//h3[@class='fl']/text()")[0].strip()
        job_detail = "".join(html.xpath(
            "//dd[@class='job_bt']//text()")).strip()
        position = {
            'postion_name': postion_name,
            'company_name':company_name,
            'salary': salary,
            'city': city,
            'work_years': work_years,
            'education': education,
            'job_detail': job_detail
        }
        self.positions.append(position)
        print(position)
        print("="*40)
if __name__ == "__main__":
    spider = LagouSpider()
    spider.run()
```




















