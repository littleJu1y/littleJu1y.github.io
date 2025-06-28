# Django5框架


## Django5框架简介
Django是用python语言写的开源web开发框架，对比Flask框架，Django原生提供了更多的功能组件，一般来说，要构建大型项目可以使用Django框架，小型项目可以使用Flask框架。

Django框架内部架构使用的是MVT设计模式。

### MVC模式与MVT模式
MVC模式的定义如下：
1. M 全拼为 Model，主要封装对数据库层的访问，对数据库中的数据进行增、删、改、查操作。
2. V 全拼为 View，用于封装结果，生成页面展示的 html 内容。
3. C 全拼为 Controller，用于接收请求，处理业务逻辑，与 Model 和 View 交互，返回结果。

MVT模式的定义如下：
1. M 全拼为 Model，主要封装对数据库层的访问，对数据库中的数据进行增、删、改、查操作。
2. V 全拼为 View，与 MVC 中的 C 功能相同，接收请求，进行业务处理，返回应答。
3. T 全拼为 Template，与 MVC 中的 V 功能相同，负责封装构造要返回的 html。

## Django5安装
用pip即可安装
```python
pip install Django==5.0.1
```
在安装完成后，会多出一些文件。
1. python目录下的scripts目录下会多出一个django-admin.exe，这是Django的项目创建工具
2. lib下的site-packages目录下会多出一个django目录


## 创建项目
在cmd中输入以下指令即可
```shell
django-admin startproject test
```
项目在创建完成后将生成以下目录：
```shell
└─test_django
    │  manage.py
    │
    └─test_django
            asgi.py
            settings.py
            urls.py
            wsgi.py
            __init__.py
```
1. manage.py 项目管理命令行工具，内置多种方式与项目进行交互，包括启动项目，创建app，数据管理等等。不用修改该文件(脚手架文件)
2. settings.py 项目的配置文件，项目中的所有功能都需要在这里配置
3. urls.py 项目的路由配置，设置网站的具体网址内容
4. wsgi.py 全称python web server gateway interface，即python服务器网关接口，是python应用与web服务器之间的接口，用于django项目在服务器上的部署和上线，也不用修改，本质上来说启动server就是manage调用wsgi
5. asgi.py 开启一个asgi服务，asgi是异步网关协议接口，也不用修改

6. asgi与wsgi代码几乎一致，只不过一个是同步一个是异步。
7. 核心要修改的文件就是urls和settings.py

输入 `python manage.py help`可以查看相关的指令：
```shell
python .\manage.py help

Type 'manage.py help <subcommand>' for help on a specific subcommand.

Available subcommands:

[auth]
    changepassword
    createsuperuser

[contenttypes]
    remove_stale_contenttypes

[django]
    check
    compilemessages
    createcachetable
    dbshell
    diffsettings
    dumpdata
    flush
    inspectdb
    loaddata
    makemessages
    makemigrations
    migrate
    optimizemigration
    sendtestemail
    shell
    showmigrations
    sqlflush
    sqlmigrate
    sqlsequencereset
    squashmigrations
    startapp
    startproject
    test
    testserver

[sessions]
    clearsessions

[staticfiles]
    collectstatic
    findstatic
    runserver

```
### 如何启动项目
正如在上一个指令中所展示的那样，输入：
```shell
python manage.py runserver
```

运行成功后访问本机8000端口即可得到如下界面：
![初次运行](/pic/django/初次运行.png)

可以使用如下参数指定端口与ip
```shell
python manage.py runserver 0:0:0:0:8888
```
不过你得将这个ip添加到settings里面的allowed hosts里面。

### 快速使用Django展示数据（创建子应用）
要想将你的内容展示到网页上，需要以下三个步骤：
1. 创建子应用
2. 在子应用的视图文件views.py中编写视图函数
3. 把视图函数和url进行绑定并注册到django项目中

首先输入以下代码：
```shell
python manage.py startapp goods
```
注意这里创建app一般是在根目录下面创建。 在创建一个app后我们进入其中查看一下目录结构：
```shell
cd goods
tree

卷序列号为 5449-63C5
C:.
│  admin.py
│  apps.py
│  models.py
│  tests.py
│  views.py
│  __init__.py
│
└─migrations
        __init__.py

```
1. migrations 这是一个数据迁移文件，同步模型到数据库，一般是不动的
2. views 让我们看一下views里面有什么

```python
from django.shortcuts import render

# Create your views here.

```
你会发现里面空空如也，实际上你要在这里编写你的views。
我们写一个视图函数，在这里我先说明一下什么是视图函数，视图函数简称视图，他其实与普通的python函数并无区别，他只是可以接受web请求并返回web响应。视图层中有两个重要的对象：请求对象(request)与响应对象(HttpResponse)。


现在让我们在views里编写一个简单的视图函数：

```python
from django.shortcuts import render
from django.http.response import HttpResponse


# Create your views here.

def index(request):
    # 业务代码
    return HttpResponse("ok")

```

在写完视图之后，要编写路由，路由就在`urls.py`里面，编写这个文件：
原始文件：
```python
from django.contrib import admin
from django.urls import path

urlpatterns = [
    path('admin/', admin.site.urls),
]


```

修改为：
```python
from django.contrib import admin
from django.urls import path
from goods import views
urlpatterns = [
    path('admin/', admin.site.urls),
    path('index/', views.index),
]

```
然后你便可以通过`http://127.0.0.1:8000/index/`来访问函数的返回结果了，实际上之前的代码返回了一个“ok”，你也可以返回html格式的代码。比如：
`return HttpResponse("<h1>ok</h1>", content_type="text/html")`

视图一定要绑定路由。

子应用的名称将来会作为目录名存在，要符合python的命名规范，不要使用数字啥的。


## web开发相关网络知识


---

> Author: July  
> URL: http://localhost:1313/python%E5%BC%80%E5%8F%91/django5%E6%A1%86%E6%9E%B6/  

