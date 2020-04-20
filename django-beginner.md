Mac-django小白入门
文字内容大部分来自视频：https://www.youtube.com/watch?v=OTmQOjsl0eg&t=7813s，并作了一些修改。
# Python安装
可以参考https://www.jianshu.com/p/66d8d26cd82d
使用如下命令确认是否安装成功：

```
python3 --version
# Python 3.7.7
```

# 创建Python虚拟环境
如果要同时开发多个应用程序，那这些应用程序都会共用一个Python。不同应用需要的jinja版本可能不同。因此，每个应用可能需要各自拥有独立的Python运行环境。virtualenv就是用来为一个应用创建一套“隔离”的Python运行环境。

```
pip3 install virtualenv
# 创建一个独立的Python运行环境，命名为venv
virtualenv --no-site-packages venv
```

若出现下述错误：

```
virtualenv: error: unrecognized arguments: --no-site-packages
没有这个参数 –no-site-packages ，是 virtualenv 版本问题。升级：
pip3 install --upgrade virtualenv==16.7.9
```

命令virtualenv就可以创建一个独立的Python运行环境，参数--no-site-packages使得已经安装到系统Python环境中的所有第三方包都不会复制，得到一个不带任何第三方包的“干净”的Python运行环境。

新建的Python环境被放到当前目录下的venv目录。用pip安装的包都被安装到venv这个环境下，系统Python环境不受任何影响。

```
# 用source进入该环境
source venv/bin/activate
# 输出(venv)Mac:myproject$
# 退出该环境
deactivate
```
# 安装django
```
source venv/bin/activate
pip install django
django-admin --version
# 3.0.6
```

# 创建项目
```
mkdir projects && cd projects
django-admin startproject telusko
cd telusko
ls
# manage / setting / urls / wsgi / __init
```

manage.py是用于与该django进行交互的命令行工具集的入口，即项目管理器。通过执行python manage.py help来查看所有命令。settings.py是项目的总配置文件，里面包含了数据库、web应用、时间等配置。 urls.py是url配置文件，django项目中所有的地址页面都需要在这个文件中配置。 wsgi.py是python应用与web服务器之间的接口，英文名为Python Web Server Gateway Interface。

下述是即将用到的目录结构内容。model.py用于保存数据。views.py文件一般我们将视图函数单独的写在一个脚本文件中，一般名称为views.py，用来处理请求。templates就是放所需要的html文件，有时候它需要在settings文件中配置。static文件夹，用来放一些静态文件，比如图片，css文件，js文件等等。

# 启动服务
```
python manage.py runserver

# output
Watching for file changes with StatReloader
Performing system checks...

System check identified no issues (0 silenced). #这里可能出现You have 17 applied migrations. 忽视即可。
April 18, 2020 - 16:29:08
Django version 3.0.5, using settings 'telusko.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```

此时浏览器打开http://127.0.0.1:8000/，可以看见django页面。`CONTROL-C`停止服务。

# 下载VS Code
https://code.visualstudio.com/
打开项目目录。
setting.py详解(转自https://www.cnblogs.com/nanfeiyan/articles/9911788.html)：

```
涉及身份验证的系统都需要存储用户的认证信息，常用的用户认证方式主要为用户名和密码的方式，为了安全起见，用户输入的密码需要保存为密文形式，可采用已公开的不可逆的hash加密算法，比如SHA256, SHA512, SHA3等，对于同一密码，同一加密算法会产生相同的hash值，这样，当用户进行身份验证时，也可对用户输入的明文密码应用相同的hash加密算法，得出一个hash值，然后使用该hash值和之前存储好的密文值进行对照，如果两个值相同，则密码认证成功，否则密码认证失败。

由于密码是由用户设定的，在实际应用中，用户设置的密码复杂度可能不够高，同时不同的用户极有可能会使用相同的密码，那么这些用户对应的密文也会相同，这样，当存储用户密码的数据库泄露后，攻击者会很容易便能找到相同密码的用户，从而也降低了破解密码的难度，因此，在对用户密码进行加密时，需要考虑对密码进行掩饰，即使是相同的密码，也应该要保存为不同的密文，即使用户输入的是弱密码，也需要考虑进行增强，从而增加密码被攻破的难度，而使用带盐的加密hash值便能满足该需求。
 # 加密盐，在使用startproject开始生成
 SECRET_KEY = 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'
 
  # DEBUG配置为True的时候会暴露出一些出错信息或者配置信息以方便调试.但是在上线的时候应该将其关掉，防止配置信息或者敏感出错信息泄露.
 DEBUG = True
 
 # 是为了限定请求中的host值,以防止黑客构造包来发送请求.只有在列表中的host才能访问.强烈建议不要使用*通配符去配置,另外当DEBUG设置为False的时候必须配置这个配置.否则会抛出异常.配置模板如下:
 ALLOWED_HOSTS = []
 
 # INSTALLED_APPS是一个一元数组.里面是应用中要加载的自带或者自己定制的app包路径列表.
 INSTALLED_APPS = [
'django.contrib.admin',
'django.contrib.auth',
'django.contrib.contenttypes',
'django.contrib.sessions',
'django.contrib.messages',
'django.contrib.staticfiles',
'person_app'
]

 # web应用中需要加载的一些中间件列表.是一个一元数组.里面是django自带的或者定制的中间件包路径,需要注意顺序如下：
 MIDDLEWARE = [
      'django.middleware.security.SecurityMiddleware',
      'django.contrib.sessions.middleware.SessionMiddleware',
      'django.middleware.common.CommonMiddleware',
      'django.middleware.csrf.CsrfViewMiddleware',
      'django.contrib.auth.middleware.AuthenticationMiddleware',
      'django.contrib.messages.middleware.MessageMiddleware',
      'django.middleware.clickjacking.XFrameOptionsMiddleware',
  ]
  
   # Database
 # https://docs.djangoproject.com/en/1.11/ref/settings/#databases
 # 数据库配置，如果使用Django默认的sqlite3数据库则不需要修改
  # 更改为mysql时， 需注意版本，如果为Django2.1版本以上，mysql版本必须为5.6以上
  DATABASES = {
      'default': {
          'ENGINE': 'django.db.backends.mysql',
          'NAME': 'person',
          'USER': 'root',
          'PASSWORD': '123456',
          'HOST': 'localhost',
      }
  }
```

# django处理流程
转自：https://www.jianshu.com/p/c13438661bf1
其主要采用MTV模式：主要包括模型层、是图层层和模版层，其中：

模型层（model）：对于web应用中的数据进行构建和操作。
视图层（view）：封装处理用户的请求与返回的响应逻辑。
模板层（template） ：提供设计友好的语法来展示信息给用户。

Django框架的工作原理：
1、用manage.py runserver 启动Django服务器——启动时会载入settings.py（同目录）
2、访问url时候，Django会根据ROOT_URLCONF的设置来装载URLConf，按顺序逐个匹配URLConf里的URLpatterns
3、匹配成功，调用相关的视图（view）函数，并把HtppRequest对象作为第一个参数
4、视图函数中的方法可以选择性调用model函数进行相关处理
5、最后该view函数负责返回一个HttpResponse对象



# python3.6安装psycopg2
若直接安装`psycopg2`将出现如下错误

>  Error: pg_config executable not found.
    
    pg_config is required to build psycopg2 from source.  Please add the directory
    containing pg_config to the $PATH or specify the full executable path with the
    option:
        python setup.py build_ext --pg-config /path/to/pg_config build ...
    or with the pg_config option in 'setup.cfg'.
    If you prefer to avoid building psycopg2 from source, please install the PyPI
    'psycopg2-binary' package instead.
    For further information please check the 'doc/src/install.rst' file (also at
    <https://www.psycopg.org/docs/install.html>).
    ----------------------------------------
ERROR: Command errored out with exit status 1: python setup.py egg_info Check the logs for full command output.

因此，需要安装`pip install psycopg2-binary`


