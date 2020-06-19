# Django
## Django介绍

### 创建第一个项目

在terminal运行以下命令：

```
django-admin startproject mysite
```

这样子会创建一个名为mysite的项目，生成如下的项目结构

```
mysite/
	manage.py
	mysite/
		__init__.py
		settings.py
		urls.py
		wsgi.py
```

* manage.py：一个命令行用来与你的项目进行交互，是对django-admin.py工具的简单封装，不需要编辑
* mysite/ ：项目目录
  * init.py: 空文件用来告诉python这个mysite目录是一个python模块
  * setting.py：项目的设置和配置，包含一些初始化的设置
  * urls.py：url模式存放的地方，这里定义的每一个url都映射一个视图（view）
  * wsgi.py：配置你的项目运行如同一个WSG应用

这时默认生成的setting.py文件包含一个使用SQLite数据库的基础配置和一个Django应用列表，这些应用会默认添加到你的项目中，我们需要为这些初始应用在数据库中创建表

在terminal中执行：

```
cd mysite
python manage.py migrate
```



### 运行开发服务器

Django 自带一个轻量web服务器来运行代码，不需要额外配置服务器。一般情况下当代码发生变化时，服务器会自动重启。

在terminal中执行：

```
python manage.py runserver
```

得到：

<img src="C:\Users\Wenyue Wang\AppData\Roaming\Typora\typora-user-images\image-20200306212350599.png" alt="image-20200306212350599" style="zoom:53%;" />

这时我们的host是http://127.0.0.1:8000/ ，在浏览器中打开，会得到这样一个界面：

<img src="C:\Users\Wenyue Wang\AppData\Roaming\Typora\typora-user-images\image-20200306212003825.png" alt="image-20200306212003825" style="zoom:50%;" />

同样，可以指定Django在定制的host和端口上运行开发服务，或者告诉它想要通过读取一个不同的配置文件来运行你的项目，可以在terminal中执行：

```
python manage.py runserver 127.0.0.1:8001\
--settings=mysite.settings
```

这里在host更改为8001，配置文件可以在settings中改。



### 项目设置

打开settings.py看下项目配置

```python
DEBUG = True
```

这是一个布尔型变量用来设置是否开启项目的debug模式，如果设置为true，一般我们会为想得到的异常设置返回参数，所以当应用抛出一个未被捕获的异常时，django会显示一个详细的错误页面。当将项目投入到生产项目时，要将debug设置为false。

```python
ALLOWED_HOSTS=[]
```

一旦准备部署你的项目到生产环境并且关闭了debug模式后，为了允许访问你的Django项目，必须将域(domain)或者host加入在这个设置中。

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

这个设置告诉Django有哪些应用会在这个项目中激活

* django.contrib.admin：管理站点
* django.contrib.auth：权限框架
* django.contrib.contenttypes：内容类型的框架
* django.contrib.sessions：会话（session）框架
* django.contrib.messages：消息框架
* django.contrib.staticfiles：用来管理静态文件的框架

```
MIDDLEWARE_CLASSES
```

包含可执行中间件的元组

```
ROOT_URLCONF = 'mysite.urls'
```

指明应用定义的主URL模式存放在哪个Python模块中

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}
```

包含了所有在项目中使用的数据库的设置的字典，里面默认的数据库使用SQLite3

```
LANGUAGE_CODE = 'en-us'
```

Django站点默认语言编码



### 项目和应用

一个项目被认为是一个安装了一些设置的Django。一个应用是一个包含模型（models），视图（views），模板（templates）以及URLs的组合，应用可以被各种各样的项目重复使用。可以简单理解为项目就是你的网站，这个网站包含了多个应用，例如Blog，wiki或者论坛，这些应用都可以被其他项目使用。



### 创建一个应用

假设创建一个blog应用，在项目主目录下运行：

```
python manage.py startapp blog
```

这个命令会创建blog应用的基本目录结构：

```
blog/
    migrations/
        __init__.py
	__init__.py
    admin.py
    apps.py
    models.py
    tests.py
    views.py
```

* admin.py：在这里注册你的模型并将它们包含到Django的管理页面中，可选
* migrations：这个目录包含你的应用的数据库迁移，migrations允许Django跟踪你的模型变化并因此来同步数据库
* models.py：应用的数据模型。所有Django应用都应该拥有一个models.py文件，可为空
* tests.py：为应用创建测试
* views.py：应用逻辑。每一个视图（view）都会接受一个http请求，处理该请求，并返回一个响应
* apps.py：应用的设置和配置，包含一些初始化的设置。这个网站讲述了设置应该放置在项目里还是应用里面https://www.jb51.net/article/148849.htm



### 设计数据架构 - models

一个model就是一个python类，继承了django.db.models.model，其中的每一个属性表示一个数据库字段。Django将为models.py里面的每一个定义的model创建一张表。当创建好一个model，Django会提供一个实用的API来方便查询数据库。

%% 

https://www.jianshu.com/p/05810d38f93a