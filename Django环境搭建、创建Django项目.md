## 1 Django环境搭建
### 安装Django
- 安装方式
    - 方式一：`pip install django` （需要外网 -- 推荐使用第一种方式安装）
    - 方式二：下载.whl文件 `pip install Django-2.0.6-py3-none-any.whl`
    - 方式三：下载压缩包：
下载 Django 压缩包，解压，进入 Django 目录，执行`python setup.py install`，然后开始安装，Django将要被安装到Python的Lib下site-packages。

- **检查是否安装成功** 
cmd下进入python环境 ：
`import django `
`django.VERSION 或 django.get_version()`

##  2 创建Django项目
###  MTV开发模式
Django的MTV模式本质上和MVC是一样的，也是为了各组件间保持松耦合关系，只是定义上有些许不同，Django的MTV分别是值：
- **M 代表模型（Model）**：负责业务对象和数据库的关系映射(ORM)。M
- **T 代表模板 (Template）**：负责如何把页面展示给用户(html)。 V
- **V 代表视图（View）**：负责业务逻辑，并在适当时候调用Model和Template。C

### 步骤1：新建工程
#### 方法一：cmd命令行创建项目
 **新建一个Web框架工程**
- 使用 django-admin 来创建 mysite 项目：
`>django-admin startproject mysite`
工程结构目录结构
```python
mysite/         #外层目录，名字可以更改
|-- mysite/         #工程目录，保存代码和文件
|   |-- __init__.py         #一个将mysite定义为包的空文件
|   |-- setting.py          #部署和配置整个工程的配置文件
|   |-- urls.py         #URL路由的声明文件（路由文件）
|   |-- wsgi.py         #基于WSGI的Web服务器的配置文件
|-- manage.py           #一个与Django工程进行交互的命令工具
```
- 进入mysite文件目录下，输入以下命令，启动服务器：`python manage.py runserver`
- 在浏览器输入你服务器的ip及端口号 ：`python manage.py runserver 127.0.0.1：8000`
- 打开服务器地址网面，出现Django的页面说明创建成功。

#### 方法二：Pycharm创建项目
- 打开Pycharm，选择菜单项【File】->【New Project】->【Django】->【Location】->【Existing interprter】 

### 步骤2：修改工程
#### 步骤2-1 创建一个具体的应用（功能模块）
>**工程（project）和应用（app）什么关系？**
>工程对于一个网站，是配置和应用的集合应用对应于特定功能，是具体功能的载体，配置和功能分离是高度模块化的体现。

>项目的开发过程中，会有模块化开发，如电商系统中的用户模块，订单模块，OA系统中的财务模块，人力模块等。每个模块都是project的一个app，app内是相关模块的功能集合，包含所有相关的功能及完整的实现。将一个project划分为多个app是一个解耦的过程，整个项目结构松散，利于维护.
>一个app对应一个功能模块
- 在Pycharm的左下角的terminal中，执行如下命令（terminal中自动激活了当前项目所使用的虚拟环境）：
`python mange.py startapp helloapp`*/ /helloapp为app模块名称*
- App目录结构如下：

```
mysite
|-- helloapp
|   |-- __init__.py
|   |-- admin.py
|   |-- app.py
|   |-- models.py
|   |-- tests.py
|   `-- views.py
```
#### 步骤2-2 增加模板templates文件
模板用于分离文档的表现形式和内容。
1. 创建templates目录
在helloworld根目录下创建一个templates目录。
2. 创建html文件
打开templates目录，并创建一个index.html文件。
3. 添加html内容
在html文件中可以添加如下内容：`<h1>Hello World！</h1>`  
或导入写好的HTML页面
4. 修改settings.py指定模板templates路径
修改helloworld/settings.py，修改 TEMPLATES 中的 DIRS 为 `'DIRS': [os.path.join(BASE_DIR, 'helloapp/templates')] `，如下所示:
```python
TEMPLATES = [    
    {       
        'BACKEND': 'django.template.backends.django.DjangoTemplates', 
        # 指定templates所在路径 
        'DIRS': [os.path.join(BASE_DIR, 'helloapp/templates')],
        'APP_DIRS': True, 
        'OPTIONS': {            
            'context_processors': [               
                'django.template.context_processors.debug',               
                'django.template.context_processors.request',               
                'django.contrib.auth.context_processors.auth',                
                'django.contrib.messages.context_processors.messages', 
            ],      
        },  
    },
]
```

#### 步骤2-3 创建视图（修改应用的views.py文件创建视图）
修改views.py，使用xxx.html为返回页面
views.py中包含对某个HTTP请求（url）的响应
```python
from django.http import HttpResponse
def hello(request):   
return HttpResponse("Hello World! I am coming…")
# 小括号中的字符串最终会响应到前端页面,可识别标签
```
或
```python
from django.shortcuts import render
#render()函数 更简洁直接
def hello(request):
    return render(request,'index.html')
    # render（）是一个打包函数，第一个参数是request，第二个参数就是页面。
```


#### 步骤2-4 修改URL路由
>路由就是处理URL和函数的关联过程
1. 修改局部app（功能模块）路由
在helloapp下创建urls.py文件，并做如下修改：
```python
from django.urls import path
from . import views         #引入本文件下的views.py
urlpatterns=[    
    path(' ', views.hello)            #调用helloapp中views.py文件中的hello方法
]
```
2. 修改全局路由
```python
from django.contrib import admin
from django.urls import path
from helloapp import views          #引入views.py
# Django适用urlpatterns变量表示路由（urls.py），该变量是列表类型，由path()或re_path()作为元素组成。
urlpatterns = [         #urlpatterns变量名固定
    path('admin/', admin.site.urls),
    path('hello/', views.hello),     #调用helloapp中views.py文件中的hello方法
]
```
或
```python
from django.contrib import admin
from django.urls import path
from django.urls import include       #include()函数，用于引入其他路由文件
# Django适用urlpatterns变量表示路由（urls.py），该变量是列表类型，由path()或re_path()作为元素组成。
urlpatterns = [         #urlpatterns变量名固定
    path('hello/', include('helloapp.urls')),         #将某个app的局部路由urls.py增加到全局路由中
]
```
在urls.py中指定URL与处理函数之间的路径关系
在原有的代码基础之上，新增加对新定义应用的引用



### 步骤3：运行工程
调试运行Web框架
在mysite工程目录下执行
`python manage.py runserver` 或  `python manage.py runserver 127.0.0.1：8000` 

>**django-admin**:Django框架全局的管理工具。
>`django-admin<command>[options]:`
>* 建立并管理Django工程
>* 建立并管理Django工程使用的数据库
>* 控制调试或日志信息
>* 运行并维护Django工程
>更多功能：\\>django-admin help

>**mange.py**
>`python mange.py <command>[options]`

### 总结
步骤1：新建工程项目：使用pycharm创建一个django的项目  （选择项目使用的虚拟环境）
步骤2-1：新建应用app：
步骤2-2：增加模板，即显示界面的HTML/CSS/JavaScript代码，配置路径
步骤2-3：修改views.py文件增加视图
步骤2-4：设置URL路由，本地路由和全局路由：在urls.py文件中，为每一个视图函数提供一个访问的url
步骤2-5：编写交互代码
步骤3：运行工程
