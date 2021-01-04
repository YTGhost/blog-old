---
title: Python3网络爬虫开发实战——第一章笔记
date: 2020-06-20 18:17:56
tags:
- Python
- 爬虫
categories:
- 学习笔记
---
第一章~~
<!--more-->
## Python3的安装

由于之前我已经熟练使用Anaconda进行环境配置了，因此这里我就直接创建一个新环境。作者在写这本书时Python版本为3.6，下面配置的也是3.6。

`conda create -n WebSpider python=3.6`

创建环境后切换到该环境

`conda activate WebSpider`

现在我们就创建好环境并切换到该环境了。

## 请求库的安装

这里我们可以直接用conda来安装，就不用pip了。

### requests的安装

`conda install requests`

### Selenium的安装

Selenium是一个自动化测试工具，利用它我们可以驱动浏览器执行特定的动作，如点击、下拉等操作。对于一些JavaScript渲染的页面来说，这种抓取方式非常有效。

`conda install selenium`

### ChromeDriver的安装

还需安装ChromeDriver驱动，先查看Chrome版本号，根据版本号在官网（下面链接）中找到对应的驱动。

官网：

`https://chromedriver.chromium.org/`

驱动版本库：

`http://chromedriver.storage.googleapis.com/index.html`

下载完后将对应的exe文件放入环境中scripts文件夹即可，目录一般如下：

`Anaconda3\envs\WebSpider\Scripts`

放完后在命令行该环境下输入**chromedriver**，若出现如下输出，即配置成功。

![](https://image.hihia.top/Screenshot/20200620143025.png)

可再运行如下代码，若能弹出空白的Chrome页面，即配置无误。

```python
from selenium import webdriver
browser = webdriver.Chrome()
```

### GeckoDriver的安装

这里因为使用的Chrome，如果用的Firefox要安装这个驱动，这里就不再赘述。

在`https://github.com/mozilla/geckodriver`下载发行版本，将其中exe放入Scripts文件夹中即可。

### PhantomJS的安装

PhantomJS是一个无界面的、可脚本编程的WebKit浏览器引擎，它原生支持多种Web标准：DOM操作、CSS选择器、JSON、Canvas以及SVG。使用它我们在运行的时候就不会像之前测试Selenium时一样弹出浏览器了。

在如下网址中选择对应系统版本进行下载：

`https://phantomjs.org/download.html`

将下载好的压缩包中bin文件夹下的exe文件如之前一样放入Scripts文件夹即可，查看版本正确显示即配置成功。

![](https://image.hihia.top/Screenshot/20200620143034.png)

这里运行如下代码进行测试时遇到警告：

```python
from selenium import webdriver
browser = webdriver.PhantomJS()
browser.get('https://www.baidu.com')
print(browser.current_url)
```

`UserWarning: Selenium support for PhantomJS has been deprecated, please use headless versions of Chrome or Firefox instead
  warnings.warn('Selenium support for PhantomJS has been deprecated, please use headless '`

错误显示当前的Selenium版本已经不支持PhantomJS，我们需要对Selenium进行降级。

通过`conda list`查看当前Selenium版本为`3.141.0`，我选择降级到作者使用的`3.4.3`版本，输入以下命令即可：

`conda install selenium=3.4.3`

降级后运行代码，由于我们这里访问的是百度，因此会将当前百度的URL输出出来，这样便测试成功。

### aiohttp的安装

之前的requests库是一个阻塞式HTTP请求库，当我们发出一个请求后，程序会一直等待服务器响应，直到得到响应后，程序才会进行下一步处理。而aiohttp库就提供了异步的Web服务，这样会大大提高我们抓取的效率。

输入以下命令安装：

`conda install aiohttp`

## 解析库的安装

抓取网页代码后，我们需要在网页中提取信息，用正则表达式则太过繁琐，我们可以使用一些解析库来高效便捷地从网页中提取信息。

### lxml的安装

lxml是Python的一个解析库，支持HTML和XML的解析，支持XPath解析方式，而且解析效果非常高。

输入以下命令安装：

`conda install lxml`

### Beautiful Soup的安装

Beautiful Soup是Python的一个HTML或XML的解析库，我们可以用它来方便地从网页中提取数据。

`conda install beautifulsoup4`

我们可以通过如下代码来验证安装，如下代码提取了<p></p>中的内容

```python
from bs4 import BeautifulSoup
soup = BeautifulSoup('<p>Hello</p>', 'lxml')
print(soup.p.string)
```

### pyquery的安装

pyquery同样是一个强大的网页解析工具，它提供了和jQuery类似的语法来解析HTML文档，支持CSS选择器，使用非常方便。

输入以下命令安装：

`conda install pyquery`

### tesserocr的安装

在爬虫过程中，难免会遇到各种各样的验证码，而大多数验证码还是图形验证码，这时候我们可以直接用OCR来识别。

tesserocr是Python的一个OCR识别库，但其实是对tesseract做的一层Python API封装，所以它的核心是tesseract。因此，在按照tesserocr之前，我们需要安装tesseract。

在如下页面选择下图所示版本进行下载和安装：

`https://digi.bib.uni-mannheim.de/tesseract/`

![](https://image.hihia.top/Screenshot/20200620143038.png)

安装时可以勾选`Additional language data(download)`选项来安装OCR识别的语言包。

接下来再安装tesserocr即可，输入如下命令安装：

`conda install tesserocr pillow`

下面可以分别测试tesseract和tesserocr

使用命令行运行如下命令，可以将识别结果保存在result.txt文件中

`tesseract image.png result -l eng`

使用python代码来测试tesserocr

```python
import tesserocr
from PIL import Image
image = Image.open('image.png')
print(tesserocr.image_to_text(image))

# 或者可以这样
import tesserocr
print(tesserocr.file_to_text('image.png'))
```

## 数据库的安装

作为数据存储的重要部分，数据库同样是必不可少的，这里我们使用的数据库主要有关系型数据库MYSQL及非关系型数据库MongoDB、Redis

### MySQL的安装

这里本地和服务器端都已经安装过，不再赘述。

### MongoDB的安装

MongoDB是由C++语言编写的非关系型数据库，是一个基于分布式文件存储的开源数据库系统，其内容存储形式类似JSON对象，它的字段值可以包含其他文档、数组及文档数组，非常灵活。

**Windows下安装**：

在如下网址下载并安装：

`https://www.mongodb.com/try/download/community`

<img src="https://image.hihia.top/Screenshot/20200620145147.png" alt="" style="zoom: 67%;" />

配置环境变量，将如下路径加入到系统变量PATH中：

`C:\Program Files\MongoDB\Server\4.2\bin`

安装完，这里我们将MongoDB设置为系统服务。以管理员模式运行命令行，在命令行输入如下内容，运行无错误即安装成功：

`mongod --bind_ip 0.0.0.0 --logpath "D:\Program Files\MongoDB\Server\4.2\log\mongodb.log" --logappend --dbpath "D:\Program Files\MongoDB\Server\4.2\data" --port 27017 --serviceName "MongoDB" --serviceDisplayName "MongoDB" --install`

我们可以在计算机管理页面查看到服务，如下图：

![](https://image.hihia.top/Screenshot/20200620155401.png)

**CentOS下安装**（目前错误，暂未解决服务开启不了的问题，请忽略这一板块）：

首先先添加MongoDB源：

`sudo vim /etc/yum.repos.d/mongodb-org.repo`

添加如下内容并保存：

```shell
[mongodb-org-4.0]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/#releasever/mongodb-org/4.0/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-4.0.asc

```

执行yum命令进行安装

`sudo yum install mongodb-org `

运行MongoDB:

`mongod --port 27017 --dbpath /data/db`

首先启动MongoDB服务：

`sudo systemctl start mongod`

进入MongoDB命令行

`mongo --port 27017`

在命令行中进行如下操作，来创建管理员用户：

![](https://image.hihia.top/Screenshot/20200620152625.png)

修改MongoDB的配置文件，执行如下命令：

`sudo vim /etc/mongod.conf`

修改net部分为如下图所示，默认是`127.0.0.1`，这样就只限于本机连接，修改后即可远程连接，从这里我们也可以看到服务监听的端口为`27017`：

![](https://image.hihia.top/Screenshot/20200620151014.png)

以及添加权限认证：

![](https://image.hihia.top/Screenshot/20200620152828.png)

### 1.4.3 Redis的安装

**Windows下安装：**

进入如下网址下载msi安装即可：

`https://github.com/microsoftarchive/redis/releases`

**CentOS下安装：**

**暂未安装**

## 存储库的安装

我们之前安装了数据库，但现在我们需要让数据库和Python交互的话，得安装一些存储库，如MySQL需要安装PyMySQL，MongoDB需要安装PyMongo

### PyMySQL的安装

输入如下命令即可：

`conda install pymysql`

### PyMongo的安装

输入如下命令即可：

`conda install pymongo`

### redis-py的安装

输入如下命令即可：

`conda install redis`

### RedisDump的安装

RedisDump是一个用于Redis数据导入/导出的工具，是基于Ruby实现的，所以要安装RedisDump，需要先按照Ruby。

安装Ruby教程在该网址`http://www.rubylang.org/zh_cn/documentation/installation/`

## Web库的安装

我们需要Web服务程序来搭建一些API接口，供我们的爬虫使用。例如，维护一个代理池，代理保存在Redis数据库中，我们要将代理池作为一个公共的组件使用，通过Web服务提供一个API接口，我们只需要请求接口即可获取新的代理。

### Flask的安装

Flask是一个轻量级的Web服务程序，它简单、易用、灵活，这里主要用来做一些API服务。

输入以下命令安装：

`conda install flask`

可以使用如下代码进行测试，运行后会在5000端口开启Web服务

```python
from flask import Flask
app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello World!"

if __name__ == "__main__":
    app.run()
```

### Tornado的安装

Tornado是一个支持异步的Web框架，通过使用非阻塞I/O流，它可以支撑成千上万的开放连接，效率非常高。

输入以下命令安装：

`conda install tornado`

可运行如下代码进行测试，运行后会在8888端口开启Web服务

```python
import tornado.ioloop
import tornado.web

class MainHandler(tornado.web.RequestHandler):
    def get(self):
        self.write("Hello, world")

def make_app():
    return tornado.web.Application([
        (r"/", MainHandler),
    ])

if __name__ == "__main__":
    app = make_app()
    app.listen(8888)
    tornado.ioloop.IOLoop.current().start()
```

## App抓取相关库的安装

**该部分暂时不弄，等学到APP抓取会补充~~**

除了Web网页，爬虫也可以抓取App的数据。App中的页面要加载出来，首先需要获取数据，而这些数据一般是通过请求服务器的接口来获取的。由于App没有浏览器这种可以比较直观地看到后台请求的工具，所以主要用一些抓包技术来抓取数据。

一些简单的接口可以通过Charles或mitmproxy分析，找出规律，然后直接用程序模拟来抓取了。但是如果遇到更复杂的接口，就需要利用mitmdump对接Python来对抓取到的请求和响应进行实时处理和保存。另外，既然要做规模采集，就需要自动化App的操作而不是人工去采集，所以这里还需要一个工具叫做Appium，它可以像Selenium一样对App进行自动化控制，如自动化模拟App的点击、下拉等操作。

### Charles的安装

到如下网页下载安装即可：

`https://www.charlesproxy.com/download/`

## 爬虫框架的安装

框架能帮我们更加高效得进行开发

### pyspider的安装

输入如下命令即可：

`pip install pyspider`(conda似乎没有？)

安装完成后输入`pyspider all`运行

出现报错：`ValueError: Invalid configuration:
  \- Deprecated option 'domaincontroller': use 'http_authenticator.domain_controller' instead.`

经查询是因为WsgiDAV发布了版本 pre-release 3.x。

解决方案：

找到库文件夹中webui文件夹下的webdav.py文件，

路径可能类似这样：`Anaconda3\envs\WebSpider\Lib\site-packages\pyspider\webui`

webdav.py文件中第209行：

```python
'domaincontroller': NeedAuthController(app),
```

将其修改为：

```python
'http_authenticator':{
        'HTTPAuthenticator':NeedAuthController(app),
    },
```

问题可解决，其会在5000端口开启服务。

### Scrapy的安装

输入如下命令即可：

`conda install Scrapy`

### Scrapy-Splash的安装

**后续用到再安装~~**

### Scrapy-Redis的安装

**后续用到再安装~~**

## 部署相关库的安装

**学到分布式爬虫再弄~~**