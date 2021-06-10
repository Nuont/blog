---
title: Django
date: 2021-06-07 10:30:04
author: L
summary: Django
categories: 框架
tags:
  - Django
---

## 设计模式

* MVC模式

  有一种程序设计模式叫**MVC**，其核心思想是**分工、解耦，让不同的代码块之间降低耦合，增强代码的可扩展性和可移植性，实现向后兼容**

  * M全拼为Model，主要封装对数据库层的访问，对数据库中的数据进行增、删、改、查操作
  * V全拼为View，用于封装结果，生成页面展示的html内容
  * C全拼为Controller，用于接收请求，处理业务逻辑，与Model和View交互，返回结果

* MVT模式

  * M去全拼为Model，与MVC中的M功能相同，负责和数据库交互，进行数据处理

  * V的全拼为View，与MVC中的C功能相同，接收请求，进行业务处理，返回应答
  * T全拼为Template，与MVC中的V功能相同，负责封装构造要返回的html

## Django流程

* 创建工程

  创建工程的命令为：

  ```python
  django-admin startproject 工程名称
  ```

  创建子应用

  ```python
  python manage.py startapp 子应用名称
  ```

  运行开发服务器

  ```
  python manage.py runserver ip:端口
  或:
  python manage.py runserver
  # 可以不写IP和端口，默认IP是127.0.0.1，默认端口是8000
  ```

  **项目结构分析:**

  1. `manage.py`：以后和项目交互基本上都是基于这个文件，一般都是在终端输入python manage.py [子命令]	可以输入python manage.py help 看下能做什么事，除非你知道你自己在做什么，一般情况下不会编辑这个文件
  2. `setting.py`：保存项目所有的配置信息
  3. `urls.py`：用来做url和视图函数映射的，以后来了一个请求，就会从这个文件中找到匹配的视图函数
  4. `wsgi.py`：专门用来做部署的，不需要修改

  * **app结构分析:**

    admin.py文件跟网站的后台管理站点配置相关

    apps.py文件用于配置当前子应用的相关信息

    migrations目录用于存放数据库迁移历史文件

    model.py文件用于保存数据库模型类

    tests.py文件用于开发测试用例，编写单元测试

    view.py文件用于编写Web应用视图

  * 3. 注册安装子应用

    创建出来的子应用目录文件虽然被放到了工程项目目录中，但是django工程并不能立即直接使用该子应用，需要注册安装后才能使用。

    在工程配置文件settings.py中，**INSTALLED_APPS**项保存了工程中已经注册安装的子应用，

    **注册安装一个子应用的方法，即是将子应用的配置信息文件apps.py中的Config类添加到INSTALLED_APPS列表中。**

  **DEBUG模式:**

  1. 如果开启了DEBUG模式，那么以后我们修改了Django项目的代码，然后按下ctrl+s，那么Django就会自动的给我们重启项目，不需要手动重启。
  2. 如果开启了DEBUG模式，那么以后Django项目中的代码出现了bug，在浏览器和控制台中会打印出错信息。
  3. 在生产环境中，禁止开启DEBUG模式，不然会有很大的安全隐患
  4. 如果将DEBUG设置为False，那么必须要设置ALLOWED_HOSTS。

  **ALLOWED_HOSTS:**

  这个变量是用来设置以后别人只能用通过这个变量中的ip地址或者域名进行访问。

  **让同局域网中的其他电脑访问本机的项目:**

  1. 让项目运行的时候, host为0.0.0.0

     * 在终端，使用命令: `python manage.py runserver 0.0.0.0:8000`
     * 在pycharm，右上角 -> 项目配置 -> host，改为`0.0.0.0`

  2. 在`setting.py`文件中，配置`ALLOWED_HOSTS`，

     将本机的IP地址添加进去。示例代码如下:

     ```python
     ALLOWED_HOSTS = ['192.168.0.103']
     ```

     注意: 要关闭自己电脑的防火墙才行。

### 模型

使用Django进行数据库开发的提示:

1. MVT设计模式中的model，专门负责和数据库交互对应(model.py)
2. 由于Model中内嵌了ORM框架，所有不需要直接面向数据库编程
3. 而且定义模型类，通过模型类和对象完成数据表的增删改查
4. ORM框架就是把数据库表的行与相应的对象建立关联，互相转换，使得数据库的操作面向对象

使用Django进行数据库开发的步骤：

1. 定义模型类

   - 根据书籍表结构设计模型类:
     - 模型类：BookInfo
     - 书籍名称字段：name
   - 根据人物表结构设计模型类：
     - 模型类：PeopleInfo
     - 人物姓名字段：name
     - 人物性别字段：gender
     - 外键约束：book
       - 外键要指定所属的模型类`book = models.ForeignKey(BookInfo)`
   - 说明 :
     - 书籍-人物的关系为一对多. 一本书中可以有多个英雄.
     - 不需要定义主键字段, 在生成表时会自动添加, 并且值为自增长.
   - 根据数据库表的设计
     - 在`models.py`中定义模型类,继承自`models.Model`

   ```python
   from django.db import models
   
   # Create your models here.
   # 准备书籍列表信息的模型类
   class BookInfo(models.Model):
       # 创建字段，字段类型
       name = models.CharField(max_length=10)
   
   # 准备人物列表的信息的模型类
   class PeopleInfo(models.Model):
       name = models.CherField(max_length=10)
       gender = models.BooleanField()
       # 外键约束：人物属于哪本书
       book = models.Foreignkey(BookInfo)
   ```

2. 模型迁移(建表)

   迁移由两步完成:

   * 生成迁移文件：根据模型类生成创建表的语句

   ```python
   python manage.py makemigrations
   ```

   * 执行迁移: 根据第一步生成的语句在数据库中建表

   ```python
   python manage.py migrate
   ```

   - **提示：默认采用**`sqlite3`**数据库来存储数据**

3. 操作数据库

### 站点管理

站点: 分为`内容发布`和`公共访问`两部分

内容发布的部分由网站的管理员负责查看、添加、修改、删除数据

Django能够根据定义的模型类自动地生成管理模块

使用django的管理模块, 需要按照如下步骤操作 :

​	1.管理界面本地化

```python
# 在settings.py文件中
# 设置中文
LANGUAGE_CODE = 'zh-Hans'
# 设置时区
TIME_ZONE = 'Asia/Shanghai'
```

​	2.创建管理员

```python
# 创建管理员命令
python manage.py createsuperuser

# 重置密码
python mamage.py changepassword 用户名
```

​	3.注册模型类

```python
# 在子应用的admin.py文件中注册模型类
# 需要导入模型模块: from book.models import BookInfo,PeopleInfo
from book.models import BookInfo,PeopleInfo

# 注册书籍模型类
admin.site.register(BookInfo)
# 注册人物模型类
admin.site.register(PeopleInfo)
```

​	4.发布内容到数据库

​	发布内容后，优化模型类展示

```python
# 准备书籍列表信息的模型类
class BookInfo(models.Model):
    # 创建字段，字段类型...
    name = models.CharField(max_length=10)

    def __str__(self):
        """将模型类以字符串的方式输出"""
        return self.name
```

### 视图和URL

站点管理页面做好了，接下来就要做公共访问页面

对应Django的设计框架MVT

​	用户在URL中请求的是视图

​	视图接收请求后进行处理

​	并将处理的结果返回给请求者

使用视图时需要进行两步操作

1. 定义视图

视图就是一个Python函数，被定义在应用的views.py中，

视图的第一个参数是HttpRequest类型的对象，包含了所有请求信息

视图必须返回HttpResponse对象，包含返回给请求者的响应信息

需要导入HttpResponse模块: from django.http import HttpResponse

```python
# 定义视图函数:响应字符串ok!给客户端
from django.http import HttpResponse

# 定义视图函数
def index(request):
    return HttpResponse('ok!')
```

2. 配置URLconf

查找视图的过程:

1. 请求者在浏览器地址栏中输入URL，请求到网站
2. 网站获取URL信息
3. 然后与编写好的URLconf逐条匹配
4. 如果匹配成功则调用对应的视图
5. 如果所有的URLcinf都没有匹配成功，则返回404数据

需要两步完成URLconf配置

1. 在项目中定义URLconf

   ```python
   from django.conf.urls import url, include
   
   urlpatterns = [
       url(r"^", include('book.urls'))
   ]
   ```

2. 在应用中定义URLconf

   一条URLconf包括URL规则、视图两部分

   * URL规则使用正则表达式定义
   * 视图就是在view.py中定义的视图函数

   ```python
   from book.views import index
   
   urlpatterns = [
       url(r'^index/$', index) # 路由引导视图函数
   ]
   ```

**url中添加参数**

1. 采用url中使用变量的方式： 在path的第一个参数中，使用`<参数名>`的方式可以传递参数，然后在视图函数中也要写一个参数，视图函数中的参数必须和url中的参数名称保持一致，不然就找不到这个参数。另外，url中可以传递多个参数。

2. 采用查询字符串的方式：在url中，不需要单独的匹配查询字符串的部分，只需要在视图函数中使用`request.GET.get(参数名称)`的方式来获取。示例代码如下:

   ```python
   def author_detail(request):
       author_id = request.GET['id']
       text = '作者的id是:%s' % author_id
       return HttpResponse(text)
   ```

   因为查询字符串使用的是`GET`请求，所以我们通过`request.GET`来获取参数，并且因为`GET`是一个类似于字典的数据类型，所以获取值跟字典的方式都是一样的。

**path函数：**

`path`函数的定义为:`path(route, view, name=None, kwarg=None)`。以下是对几个参数进行讲解：

1. `route`参数：`url`的匹配规则，这个参数中可以指定`url`中需要传递的参数，比如在访问文章详情页的时候，可以传递一个`id`。传递参数是通过`<>`括号来进行指定的。并且在传递参数的时候，可以指定这个参数的类型，比如文章的`id`都是`int`类型，那么可以这样写`<int:id>`，以后匹配的时候，就只会匹配到id为`int`类型的`url`，而不会匹配其他的`url`，并且在视图函数中获取这个参数的时候，就已经被转换成一个`int`类型了。其中还有几个常用的类型：
   * str:非空的子字符类型。默认的转换器。但是不能包含斜杠
   * int: 匹配任意的零或者正数的整数，到视图的函数中就是一个int类型
   * slug: 由英文的横杠`-`，或者下划线`_`连接英文字符或者数字而成的字符串
   * uuid: 匹配`uuid`字符
   * path: 匹配非空的英文字符串，可以包含斜杠
2. `view`参数：可以为一个视图函数或者是`类视图.as_view()`或者是`django.urls.include()`函数的返回值
3. `name`参数：这个参数是给这个`url`取个名字的，这在项目比较大，`url`比较多的时候用处很大。
4. `kwargs`参数：有时候想给视图函数传递一些额外的参数，就可以通过`kwargs`参数进行传递。这个参数接收一个字典。传到视图函数中的时候，会作为一个关键字参数传过去，比如以下的`url`规则:

```python
from django.urls import path
from . import views

urlpatterns = [
    path('blog/<int:year>/', views.year_archive, {'foo':'bar'})
]
```

**URL参数的转换器**

1. str：除了斜杠`/`以外的所有的字符都是可以的。
2. int：只有一个或者多个阿拉伯数字。
3. path：所有的字符都是满足的。
4. uuid：只有满足`uuid.uuid4()`这个函数返回的字符串的格式。 
5. slug：英文中的横杠或者英文字符或者阿拉伯数字或者下划线才满足。

**urls模块化**

如果项目变得越来越大，那么url会变的越来越多，如果都放在主`urls.py`文件中，那么将不太好管理，因此我们可以将每个app自己的urls放到自己的app中进行管理。一般我们会在app中新建一个urls.py文件用来存储所有和这个app相关的子url。

需要注意的地方:	

1. 应该使用`include`函数包含子`urls.py`，并且这个`urls.py`的路径是相对于项目的路径。示例代码如下:

   ```python
   urlpatterns = [
       path('admin/', admin.site.urls),
       path('book/',include('book.urls'))
   ]
   ```

2. 在`app`的`urls.py`中，所有的url匹配也要放在一个叫做`urlpatterns`的变量中，否则找不到。

3. `url`是会根据主`urls.py`和app中的`urls.py`进行拼接的，因此注意不要多加斜杠。

**URL命名**

**为什么需要给url命名**

因为url是经常变化的，如果在代码中写死可能会经常改代码，给url取个名字，以后使用url的时候就使用他的名字进行反转就可以了，就不需要写死url了。

**如何给一个url指定名称**

在`path`函数中，传递一个`name`参数就可以指定。示例代码如下:

```python
urlpatterns = [
    path('', views.index, name='index'),
    path('login/', views.login, name='login')
]
```

**应用命名空间**

在多个app之间，有可能产生同名的url。这时候为了避免反转url的时候产生混淆，可以使用应用命名空间，

来做区分，定义应用命名空间非常简单，只要在`app`的`urls.py`中定义一个叫做`app_name`的变量，来指定这个应用的命名空间即可。示例代码如下:

```python
# 应用命名空间
app_name = 'front'

urlpatterns = [
    path('', views.index, name='index'),
    path('login/', views.login, name='login')
]
```

以后在做反转的时候就可以使用`应用命名空间:url名称`的方式进行反转，示例代码如下:

```python
login_url = reverse('front:login')
```

**应用(app)命名空间和实例命名空间**

一个app，可以创建多个实例。可以使用多个url映射同一个app。所以这就会产生一个问题。以后再做反转的时候，如果使用应用命名空间，那么就会发生混淆。为了避免这个问题，我们可以使用实例命名空间。实例命名空间也是非常简单，只要再`include`函数中传递一个`namespace`变量即可。示例代码如下:

```python
urlpatterns = [
    path('', include('front.urls')),
    # 同一个app下有两个实例
    path('cms1/', include('cms.urls', namespace='cms1')),
    path('cms2/', include('cms.urls', namespace='cms2')),
]
```

以后在做反转的时候，就可以根据实例命名空间来指定具体的url。示例代码如下:

```python
def index(request):
    username = request.GET.get("username")
    if username:
        return HttpResponse('CMS首页')
    else:
        # 获取当前命名空间
        current_namespace = request.resolver_match.namespace
        return redirect(reverse("%s:login"%current_namespace))
```

**include函数的用法**

1. include(module, namespace=None):

   * module：子url的模块字符串。
   * namespace：实例命名空间。这个地方需要注意一点。如果指定了实例命名空间，那么前提必须要先指定应用命名空间。也就是在子`urls.py`中添加`app_name`这个变量。

2. include((pattern_list, app_namespace), namespace=None):

   `include`函数的第一个参数既可以为一个字符串，也可以为一个元组，如果是元组，	那么元组的第一个参数是子`urls.py`模块的字符串，元组的第二个参数是应用命名空间，也就是说，应用命名空间既可以在子`urls.py`中通过`app_name`指定，也可以在`include`函数中指定。

3. include(pattern_list):

   `pattern_list`是一个列表，这个列表中装的是`path`或者`re_path`函数。实例代码如下

   ```python
   path('movie/', include([
       path('', views.movie),
       path('list/', views.movie_list), 
   ]))
   ```

**re_path函数**

1. re_path和path的作用都是一样的，只不过`re_path`是在写url的时候可以用正则表达式，功能更加强大

2. 写正则表达式都推荐使用原生字符串，也就是以`r`开头的字符串

3. 在正则表达式中定义变量，需要用圆括号括起来。这个参数有名字的，那么需要使用`?p<参数的名字>`

   然后在后面添加正则表达式的规则。示例代码如下:

   ```python
   from django.urls import re_path
   from . import views
   
   urlpatterns = [
       # r"":代表原生字符串(raw)
       re_path(r'^$', views.article),
       # /article/list/<year>/
       re_path(r'list/(?P<year>\d{4})/$', views.article_list),
       re_path(r'list/(?P<month>\d{2})/$', views.article_list_month)
   ]
   ```

4. 如果不是特别要求，直接使用`path`就够了，省的把代码搞的很麻烦(因为正则 表达式其实是非常晦涩的, 特别是一些比较复杂的正则表达式，今天写的明天就不记得了)。除非是url中确实是需要使用正则表达式来解决才使用`re_path`

**reverse函数**

1. 如果在反转url的时候，需要添加参数，那么可以传递`kwargs`参数到`reverse`函数中，实例代码如下：

   ```python
   detail_url = reverse('detail', kwargs={'article_id':1, 'page':2})
   ```

2. 如果想要添加查询字符串的参数，则必须手动的进行拼接。示例代码如下:

   ```python
   login_url = reverse('login')+'?next=/'
   ```

**自定义URL(PATH)转换器**

需求:
实现一个获取文章列表的demo，用户可以根据`/articles/文章分类`的方式来获取文章，其中文章的分类采用的是`分类1+分类2+分类3...`的方式拼接，并且如果只个一个分类，那么就不需要加号。示例如下:

```python
# 1. 第一种: 获取python分类下的文章
/articles/python/
# 2. 第二种：获取python和django分类下的文章
/articles/python+django/
# 3. 第三种：获取python和django和flask分类下的文章
/articles/python+django+flask/
以此类推...
```

在"文章分类"参数传到视图函数之前要把这些分类分开来存储到列表中。比如参数是`python+django`，那么传到视图函数的时候就要变成`['python', 'django']`。以后在使用reverse反转的时候，限制传递"文章分类"的参数应该是一个列表，并且要将这个列表变成`python+django`的形式。

**自定义URL转换器**

之前已经学过一些django内置的url转化器，包括int，uuid等，有时候这些内置的url转换器并不能满足我们的需求，因此django给我们提供了一个接口可以让我们自己定义自己的url转换器

自定义url转换器按照以下五个步骤来走就可以了:

1. 定义一个类，直接继承自object就可以了。
2. 在类中定义一个属性regex，这个属性用来保存url转换器的正则表达式。
3. 实现to_python(self, value)方法，这个方法是将url中的值转换以下，然后传给视图函数的。
4. 实现to_url(self，value)方法，这个方法是在做url反转的时候，将传进来的参数转换后拼接成一个正确的url。
5. 将定义好的转换器，使用`django.urls.converters.register_converter`方法注册到django中

```python
from django.urls.converters import register_converter

class CategoryConverter(object):
    regex = r'\w+|(\w+\+\w+)+'
    
    def to_python(self, value):
        # value: python+django+flask
        # ['python'+'django'+'flask']
        result = value.split('+')
        return result
    
    def to_value(self, value):
        # value: ['python'+'django'+'flask']
        # python+django+flask
        if isinstance(value, list):
            result = "+".join(value)
            return result
        else:
            raise RuntimeError('转换url的时候, 分类参数必须为列表！')
            
register_converter(CategoryConverter, 'cate')
```

**URL映射的时候指定默认参数**

使用path或者re_path的时候，在route中都可以包含参数，而有时候想指定默认的参数，这时候j就可以通过以下方式来完成。示例代码如下：

```python
# urls.py
from django.urls import path
from . import view

urlpatterns = [
    path('blog/', views.page),
    path('blog/page<int:num>/', views.page),
]

# View (in blog/views.py)
def page(request, num=1):
    # Output the appropriate page of blog entries, according to num.
    ...
```

当在访问blog/的时候，因为没有传递num参数，所以会匹配到第一个url，这时候就执行view.page这个视图函数，而在page函数中，又有num=1这个默认参数。因此这时候就可以不用传递参数。而如果访问blog/1	的时候，因为在传递参数的时候传递了num，因此会匹配到第二个url，这时候也会执行views.page，然后把传递进来的参数传给page函数中的num。

### 模板

在Django中，将前端的内容定义在模板中，然后再把模板交给视图调用

模板使用步骤

1. 创建模板
   * 在应用同级目录下创建模板文件templates，文件名称固定写法
   * 在templates文件夹下，创建应用同名文件夹，
   * 在应用同名文件夹下创建网页模块文件
2. 设置模板查找路径
3. 模板接收视图传入的数据
4. 模板处理数据

## 模型

在settings.py中保存了数据库的连接配置信息，Django默认初始配置使用**sqlite**数据库。

1. 使用mysql数据库首先需要安装驱动程序

   ```python
   pip install pymysql
   ```

2. 在Django的工程同名子目录的\__init__.py文件中添加如下语句

   ```python
   import pymysql
   
   pymysql.install_as_MySQLdb()
   ```

3. 修改databases配置信息

   ```python
   DATABASES = {
       'default': {
           # 数据库引擎（是mysql还是oracle等）
           'ENGINE': 'django.db.backends.mysql',
           # 数据库的名字
           'NAME': 'dfz',
           # 连接mysql数据库的用户名
           'USER': 'root',
           # 连接mysql数据库的密码
           'PASSWORD': 'root',
           # mysql数据库的主机地址
           'HOST': '127.0.0.1',
           # mysql数据库的端口号
           'PORT': '3306',
       }
   }
   ```

4. 在mysql中创建数据库

   ```python 
   create database book charset=utf8;
   ```

- **在Django中操作数据库:**

在`Django`中操作数据库有两种方式。第一种方式就是使用原生`sql`语句操作，第二种就是使用`ORM`模型来操作。在`Django`中使用原生`sql`语句操作其实就是使用`python db api`的接口来操作。如果你的`mysql`驱动使用的是`pymysql`，那么你就是使用`pymysql`来操作的，只不过`Django`将数据库连接的这一部分封装好了，我们只要在`settings.py`中配置好了数据库连接信息后直接使用`Django`封装好的接口就可以操作了。示例代码如下： 

```python
# 使用django封装好的connection对象，会自动读取setting.py中数据库的配置信息
from django.db import connection

# 获取游标对象
cursor =connection.cursor()
# 拿到游标对象后执行sql语句
cursor.execute("select * from book")
# 获取所有的数据
rows = cursor.fetchall()
# 遍历查询到的数据
for row in rows:
	print(row)
```

以上的`execute`以及`fetchall`方法都是`Python DB API`规范中定义好的。任何使用`Python`来操作`MySQL`的驱动程序都应该遵循这个规范。所以不管是使用`pymysql`或者是`mysqlclient`或者是`mysqldb`，他们的接口都是一样的。 

**Python DB API规范下cursor对象常用接口**

1. `description`：如果`cursor`执行了查询的`sql`代码。那么读取`cursor.description`属性的时候，将返回一个列表，这个列表中装的是元组，元组中装的分别是`(name,type_code,display_size,internal_size,precision,scale,null_ok)`，其中`name`代表的是查找出来的数据的字段名称，其他参数暂时用处不大。 

2. `rowcount`：代表的是在执行了`sql`语句后受影响的行数。 

3. `close`：关闭游标。关闭游标以后就再也不能使用了，否则会抛出异常。 

4. `execute(sql[,parameters])`：执行某个`sql`语句。如果在执行`sql`语句的时候还需要传递参数，那么可以传给`parameters`参数。示例代码如下： 

   ```python
   cursor.execute("select * from article where id=%s",(1,))
   ```

5. `fetchone`：在执行了查询操作以后，获取第一条数据。 

6. `fetchmany(size)`：在执行查询操作以后，获取多条数据。具体是多少条要看传的`size`参数。如果不传`size`参数，那么默认是获取第一条数据。 

7. `fetchall`：获取所有满足`sql`语句的数据。 

- ORM模型

使用原生SQL的方式会出现的问题:

1. SQL语句重复利用率不高，越复杂的SQL语句条件越多，代码越长。会出现很多相近的SQL语句。
2. 很多SQL语句是在业务逻辑中拼出来的，如果有数据库需要更改，就要去修改这些逻辑，这会很容易漏掉对某些SQL语句的修改。
3. 写SQL时容易忽略web安全问题，给未来造成隐患。SQL注入。

`ORM`，全称`Object Relational Mapping`，中文叫做对象关系映射，通过`ORM`我们可以通过类的方式去操作数据库，而不用再写原生的SQL语句。通过把表映射成类，把行作实例，把字段作为属性，`ORM`在执行对象操作的时候最终还是会把对应的操作转换为数据库原生语句。使用`ORM`有许多优点:

1. 易用性：使用`ORM`做数据库的开发可以有效的减少重复SQL语句的概率，写出来的模型也更加直观、清晰。
2. 性能损耗小：`ORM`转换成底层数据库操作指令确实会有一些开销。但从实际的情况来看，这种性能损耗很少（不足5%），只要不是对性能有严苛的要求，综合考虑开发效率、代码的阅读性，带来的好处要远远大于性能损耗，而且项目越大作用越明显。
3. 设计灵活：可以轻松的写出复杂的查询。
4. 可移植性：`Django`封装了底层的数据库实现，支持多个关系数据库引擎，包括流行的`MySQL`、`PostgreSQL`和`SQLite`。可以非常轻松的切换数据库。

**ORM模型的创建和映射**

**创建ORM模型：**

`ORM`模型一般都是放在`app`的`models.py`文件中。每个`app`都可以拥有自己的模型。并且如果这个模型想要映射到数据库中，那么这个`app`必须要放在`settings.py`的`INSTALLED_APP`中进行安装。以下是写一个简单的书籍`ORM`模型。示例代码如下：

```python
from django.db import models
class Book(models.Model):
    name = models.CharField(max_length=20,null=False)
    author = models.CharField(max_length=20,null=False)
    pub_time = models.DateTimeField(default=datetime.now)
    price = models.FloatField(default=0)
```

以上便定义了一个模型。这个模型继承自`django.db.models.Model`，如果这个模型想要映射到数据库中，就必须继承自这个类。这个模型以后映射到数据库中，表名是模型名称的小写形式，为`book`。在这个表中，有四个字段，一个为`name`，这个字段是保存的是书的名称，是`varchar`类型，最长不能超过20个字符，并且不能为空。第二个字段是作者名字类型，同样也是`varchar`类型，长度不能超过20个。第三个是出版时间，数据类型是`datetime`类型，默认是保存这本书籍的时间。第五个是这本书的价格，是浮点类型。
还有一个字段我们没有写，就是主键`id`，在`django`中，如果一个模型没有定义主键，那么将会自动生成一个自动增长的`int`类型的主键，并且这个主键的名字就叫做`id`。

**映射模型到数据库中：**

将`ORM`模型映射到数据库中，总结起来就是以下几步：

1. 在`settings.py`中，配置好`DATABASES`，做好数据库相关的配置。
2. 在`app`中的`models.py`中定义好模型，这个模型必须继承自`django.db.models`。
3. 将这个`app`添加到`settings.py`的`INSTALLED_APP`中。
4. 在命令行终端，进入到项目所在的路径，然后执行命令`python manage.py makemigrations`来生成迁移脚本文件。
5. 同样在命令行中，执行命令`python manage.py migrate`来将迁移脚本文件映射到数据库中。

### 定义模型类

模型类被定义在"应用/model.py"文件中

模型类必须继承自Model类，位于包django.db.models中

```python
from django.db import models

# Create your models here.
# 准备书籍列表信息的模型类
class BookInfo(models.Model):
    # 创建字段，字段类型...
    name = models.CharField(max_length=20, verbose_name='名称')
    pub_date = models.DateField(null=True,verbose_name='发布日期')
    readcount = models.IntegerField(default=0, verbose_name='阅读量')
    commentcount = models.IntegerField(default=0, verbose_name='评论量')
    is_delete = models.BooleanField(default=False, verbose_name='逻辑删除')

    class Meta:
        db_table = 'bookinfo'  # 指明数据库表名
        verbose_name = '图书'  # 在admin站点中显示的名称

    def __str__(self):
        """定义每个数据对象的显示信息"""
        return self.name
```

1. 数据库表名

   模型类如果未指明表名，Django默认以**小写app应用名_小写模型类名**为数据库表名。

   可通过**db_table**指明数据库表名。

2. 关于主键

   django会为表创建自动增长的主键列，每个模型只能有一个主键列，如果使用选项设置某属性为主键列后django不会再创建自动增长的主键列。

   默认创建的主键列属性为id，可以使用pk代替，pk全拼为primary key。

3. 属性命名限制

   不能是python的保留关键字

   不允许使用连续的下划线，这是由django的查询方式决定的

   定义属性时需要指定字段类型，通过字段类型的参数指定选项

   ```python
   属性=models.字段类型(选项)
   ```

4. 字段类型

   | 类型             | 说明                                                         |
   | ---------------- | ------------------------------------------------------------ |
   | AutoField        | 自动增长的IntegerField，通常不用指定，不指定时Django会自动创建属性名为id的自动增长属性 |
   | BooleanField     | 布尔字段，值为True或False                                    |
   | NullBooleanField | 支持Null、True、False三种值                                  |
   | CharField        | 字符串，参数max_length表示最大字符个数                       |
   | TextField        | 大文本字段，一般超过4000个字符时使用                         |
   | IntegerField     | 整数                                                         |
   | DecimalField     | 十进制浮点数， 参数max_digits表示总位数， 参数decimal_places表示小数位数 |
   | FloatField       | 浮点数                                                       |
   | DateField        | 日期， 参数auto_now表示每次保存对象时，自动设置该字段为当前时间，用于"最后一次修改"的时间戳，它总是使用当前日期，默认为False； 参数auto_now_add表示当对象第一次被创建时自动设置当前时间，用于创建的时间戳，它总是使用当前日期，默认为False; 参数auto_now_add和auto_now是相互排斥的，组合将会发生错误 |
   | TimeField        | 时间，参数同DateField                                        |
   | DateTimeField    | 日期时间，参数同DateField                                    |
   | FileField        | 上传文件字段                                                 |
   | ImageField       | 继承于FileField，对上传的内容进行校验，确保是有效的图片      |

5. 选项

   | 选项        | 说明                                                         |
   | ----------- | ------------------------------------------------------------ |
   | null        | 如果为True，表示允许为空，默认值是False                      |
   | blank       | 如果为True，则该字段允许为空白，默认值是False                |
   | db_column   | 字段的名称，如果未指定，则使用属性的名称                     |
   | db_index    | 若值为True, 则在表中会为此字段创建索引，默认值是False        |
   | default     | 默认                                                         |
   | primary_key | 若为True，则该字段会成为模型的主键字段，默认值是False，一般作为AutoField的选项使用 |
   | unique      | 如果为True, 这个字段在表中必须有唯一值，默认值是False        |

   **null是数据库范畴的概念，blank是表单验证范畴的**

6. 外键

在设置外键时，需要通过**on_delete**选项指明主表删除数据时，对于外键引用表数据如何处理，在django.db.models中包含了可选常量：

- **CASCADE**级联，删除主表数据时连通一起删除外键表中数据
- **PROTECT**保护，通过抛出**ProtectedError**异常，来阻止删除主表中被外键应用的数据
- **SET_NULL**设置为NULL，仅在该字段null=True允许为null时可用
- **SET_DEFAULT**设置为默认值，仅在该字段设置了默认值时可用
- **SET()**设置为特定值或者调用特定方法
- **DO_NOTHING**不做任何操作，如果数据库前置指明级联性，此选项会抛出**IntegrityError**异常

### 迁移

1. 生成迁移文件

   ```python
   python manage.py makemigrations
   ```

2. 同步到数据库中

   ```python
   python manage.py migrate
   ```

**迁移命令**

1. makemigrations：将模型生成迁移脚本。模型所在的`app`，必须放在`settings.py`中的`INSTALLED_APPS`中。这个命令有以下几个常用选项：
   - app_label：后面可以跟一个或者多个`app`，那么就只会针对这几个app生成迁移脚本。如果没有任何的app_label，那么会检查`INSTALLED_APPS`中所有的app下的模型，针对每一个app都生成响应的迁移脚本。
   - --name：给这个迁移脚本指定一个名字。
   - --empty：生成一个空的迁移脚本。如果你想写自己的迁移脚本，可以使用这个命令来实现一个空的文件，然后自己再在文件中写迁移脚本。
2. migrate：将新生成的迁移脚本。映射到数据库中。创建新的表或者修改表的结构。以下一些常用的选项：
   - app_label：将某个`app`下的迁移脚本映射到数据库中。如果没有指定，那么会将所有在`INSTALLED_APPS`中的`app`下的模型都映射到数据库中。
   - app_label migrationname：将某个`app`下指定名字的`migration`文件映射到数据库中。
   - --fake：可以将指定的迁移脚本名字添加到数据库中。但是并不会把迁移脚本转换为SQL语句，修改数据库中的表。
   - --fake-initial：将第一次生成的迁移文件版本号记录在数据库中。但并不会真正的执行迁移脚本。
3. showmigrations：查看某个app下的迁移文件。如果后面没有app，那么将查看`INSTALLED_APPS`中所有的迁移文件。
4. sqlmigrate：查看某个迁移文件在映射到数据库中的时候，转换的`SQL`语句。

### shell工具

1. shell工具

Django的manage工具提供了**shell**命令，帮助我们配置好当前工程的运行环境（如连接好数据库等），以便可以直接在终端中执行测试python语句。

通过如下命令进入shell

```python 
python manage.py shell
```

### 数据库的增删改查

* 增加

  增加数据有两种方法

  1. save: 通过创建模型类对象，执行对象的save()方法保存到数据库中

     ```python
     from book.models import BookInfo,PeopleInfo
     
     book = BookInfo(
     	name = 'python入门',
         pub_date = '2010-1-1'
     )
     book.save()
     ```

  2. create: 通过模型类.objects.create()保存

     ```python
     PeopleInfo.objects.create(
     	name = 'itheima',
         book = book
     )
     ```

* 修改

  1. save: 修改模型类对象的属性，然后执行save()方法

     ```python
     >>> person = PeopleInfo.objects.get(name='itheima')>>> person.name = 'itcast'>>> person.save()>>> person<PeopleInfo: itcast>
     ```

  2. update: 使用模型类.objects.filter().update()，会返回受影响的行数

     ```python
     >>> PeopleInfo.objects.filter(name='itcast').update(name='传智播客')
     ```

* 删除

  删除有两种方法

  1. 模型类对象delete

     ```python
     >>> person = PeopleInfo.objects.get(name='传智播客')>>> person.delete()(1, {'book.PeopleInfo': 1})
     ```

  2. 模型类.objects.filter().delete()

     ```python
     >>> BookInfo.objects.filter(name='python入门').delete()
     (1, {'book.BookInfo': 1, 'book.PeopleInfo': 0})
     ```

* 查询

  1. 基本查询

     get查询单一结果，如果不存在会抛出模型类.DoesNotExist异常

     all查询多个结果

     count查询结果数量

  2. 过滤查询

     实现SQL中的where功能，包括:

     filter过滤出多个结果

     exclude排除掉符合条件剩下的结果

     get过滤单一结果

     过滤条件的表达语法如下:

     ```python
     属性名称__比较运算符 = 值
     
     # 属性名称和比较运算符间使用两个下划线，所以属性名不能包括多个下划线
     ```

     1. 相等：

     exact: 表示判等

     ```python
     BookInfo.objects.filter(id__exact=1)
     ```

     2. 模糊查询

     contains: 是否包含

     ```python
     BookInfo.objects.filter(name__contains='传')
     ```

     startswith, endswith: 以指定开头和结尾

     ```python
     BookInfo.objects.filter(name__endswith='部')
     ```

     3. 空查询

     isnull: 是否为null

     ```python
     BookInfo.objects.filter(name__isnull=True)
     ```

     4. 范围查询

     in: 是否包含在范围内

     ```python
     BookInfo.objects.filter(id__in=[1, 3, 5])
     ```

     5. 比较查询

     gt 大于

     gte 大于等于

     lt 小于

     lte 小于等于

     ```python
     BookInfo.objects.filter(id__gt=3)
     ```

     不等于运算符，使用exclude()过滤器

     ```python
     BookInfo.objects.exclude(id=3)
     ```

     6. 日期查询

     year, month, day, week_day, hour, minute, second: 对日期时间类型的属性进行运算

     ```python
     # 查询1980年发表的图书BookInfo.objects.filter(pub_date__year==1980)# 查询1990年1月1日后发表的图书BookInfo.objects.filter(pub_date__gt=='1990-1-1')
     ```

### F和Q对象

* F对象 : F(属性名)

  两个属性的比较使用F对象，被定义在django.db.models中

  ```python
  # 查询阅读量大于等于评论量的图书from django.db.modles import FBookInfo.objects.filter(readcount__gte=F('commentcount'))
  ```

* Q对象  : Q(属性名__运算符=值)

  **多个过滤器逐个调用表示逻辑关系，同sql语句中的where部分的and关键字**

  ```python
  # 查询阅读量大于20, 并且编号小于3的图书
  
  BookInfo.object.filter(readcount__gt=20, id__lt=3)
  BookInfo.object.filter(readcount__gt=20).filter(id__lt=3)
  ```

  **如果需要实现逻辑或or的查询，需要使用Q()对象结合|运算符**

  ```python
  # 查询阅读量大于20，或编号小于3的图书，
  BookInfo.objects.filter(Q(readcount__gt=3)|Q(id__lt=3))
  
  # Q对象使用~操作符, 表示非not
  BookInfo.objects.filter(~Q(id=3))
  ```

### 聚合函数和排序函数

* 聚合函数

  使用aggregate()过滤器调用聚合函数，聚合函数包括：Avg平均，Count数量，Max最大，Min最小，Sum求和，被定义在django.db.models中

  ```python 
  # 查询图书的总阅读量
  from django.db.models import Sum
  
  BookInfo.objects.aggregate(Sum('readcount'))
  ```

  注意: aggregate的 返回值是一个字典类型，格式如下:

  ```python
  {'属性名__聚合类小写':值}
  
  如: {'readcount__sum':126}
  ```

  使用count时一般不使用aggregate()过滤器。

  例：查询图书总数。

  ```python
  BookInfo.objects.count()
  ```

  注意count函数的返回值是一个数字。

* 排序函数

  使用order_by对结果进行排序

  ```python
  # 默认升序
  BookInfo.objects.all().order_by('readcount')
  
  # 降序
  BookInfo.objects.all().order_by('-readcount')
  ```

### 关联查询

* 由多模型类条件查询一模型类数据:

  语法如下:

  ```
  关联模型名名小写__属性名__条件运算符 = 值
  ```

  ```python
  # 查询图书,要求图书人物为'郭靖'
  book = BookInfo.objects.filter(peopleinfo__name = '郭靖')
  
  # 查询图书,要求图书中人物的描述包含'八'
  book = BookInfo.objects.filter(peopleinfo__description__contains = '八')
  ```

* 由一模型类条件查询多模型类数据:

  语法如下：

  ```python
  一模型类关联属性名__一模型类属性名__条件运算符 = 值
  ```

  ```python
  # 查询书名为"天龙八部"的所有人物
  people = PeopleInfo.objects.filter(book__name='天龙八部')
  
  # 查询图书阅读量大于30的所有人物
  people = PeopleInfo.objects.filter(book__readcount__gt = 30)
  ```

### 查询集

* 1. 概念

  Django的ORM中存在查询集的概念

  查询集，也称查询结果集、QuerySet，表示从数据库中获取的对象集合。

  当调用如下过滤器方法时，Django会返回查询集（而不是简单的列表）：

  * all() : 返回所有数据
  * filter(): 返回满足条件的数据
  * exclude(): 返回满足条件以外的数据
  * order_by(): 对结果进行排序

  对查询集可以再次调用过滤器进行过滤

  ```python
  books = BookInfo.objects.filter(readcount__gt=30).order_by('pub_date')
  ```

  也就意味着查询集可以含有零个、一个或多个过滤器。过滤器基于所给的参数限制查询的结果。

  判断某一查询集中是否有数据：

  * exists(): 判断查询集中是否有数据，如果有则返回True，没有则返回False

* 2. 两大特性
     1. 惰性执行

  创建查询集不会访问数据库，直到调用数据时，才会访问数据库，调用数据的情况包括迭代、序列化、与if合用

  例如：当执行如下语句时，并未进行数据库查询，只是创建了一个查询集books

  ```
  books = BookInfo.objects.all()
  ```

  继续执行遍历迭代操作后，才真正的进行了数据库的查询

  ```python
  for book in books:
  	print(book.name)
  ```

  2. 缓存

  使用同一个查询集，第一次使用时会发生数据库的查询，然后Django会把结果缓存下来，再次使用这个查询集时会使用缓存的数据，减少了数据库的查询次数。

  ```
  books = BookInfo.objects.all()[book.id for book in books][book.id for book in books]
  ```

* 3. 限制查询集

  可以对查询集进行取下标或切片操作，等同于sql中的limit和offset子句。

  注意: 不支持负数索引

  **对查询集进行切片后返回一个新的查询集，不会立即执行查询。**

  如果获取一个对象，直接使用[0]，等同于[0:1].get()，但是如果没有数据，[0]引发IndexError异常，[0:1].get()如果没有数据引发DoesNotExist异常。

  ```python
  >>> books = BookInfo.objects.all()[0:2]
  >>> books
  <QuerySet [<BookInfo: 射雕英雄传>, <BookInfo: 天龙八部>]>
  ```

* 4. 分页

  ```python
  # 查询数据
  books = BookInfo.objects.all()
  # 导入分页类
  from django.core.paginator import Paginator
  # 创建分页类实例
  paginator = Paginator(books, 2)
  # 获取指定页码的数据
  page_skus = paginator.page(1)
  # 获取分页数据
  total_page = paginator.num_pages
  ```

## 视图

### 视图介绍

视图介绍

* 视图就是views.py文件中的函数

* 视图的第一个参数必须为HttpRequest对象，还可能包含如下参数如:

  * 通过正则表达式组获取的位置参数
  * 通过正则表达式组获取的关键字参数

* 视图必须返回一个HttpResponse对象或子对象作为响应

  * 子对象: JsonResponse HttpResponseRedirect

* 视图负责接收Web请求HttpRequest，进行逻辑处理，返回Web响应HttpResponse给请求者

  * 响应内容可以是HTML内容，404错误，重定向，json数据

* 视图处理过程如下：

  使用视图时需要进行两步操作，两步操作不分前后

  1. 配置URLconf
  2. 在应用/view.py中定义视图

### URLconf

浏览者通过在浏览器的地址栏中输入网址请求网站

对于Django开发的网站，由哪一个视图进行处理请求，是由url匹配找到的的

* 配置URLconf

  1. setting.py中

     指定url配置

     ```
     ROOT_URLCONF = '项目.urls'
     ```

  2. 项目中的urls.py中

     匹配成功后，包含到应用的urls.py

     ```
     url(正则, include('应用.urls'))
     ```

  3. 应用中的urls.py

     匹配成功后，调用view.py对应的函数

     ```
     url(正则, views.函数名)
     ```

  4. 提示

     ```
       1. 正则部分推荐使用 r，表示字符串不转义，这样在正则
       表达式中使用 \ 只写一个就可以
     
       2. 不能在开始加反斜杠，推荐在结束加反斜杠
           正确：path/
           正确：path
           错误：/path
           错误：/path/
     
       3. 请求的url被看做是一个普通的python字符串，进行匹配时不包括域名、get或post参数
           3.1 如请求地址如下：
               http://127.0.0.1:8000/18/?a=10
           3.2 去掉域名和参数部分后，只剩下如下部分与正则匹配
               18/
     ```

### 路由命名与reverse反解析

* 1. 路由命名

  在定义路由的时候，可以为路由命名，方便查找特定视图的具体路径信息。

  1) 在使用include函数定义路由时，可以使用namespace参数定义路由的命名空间，如:

  ```
  url(r'^',include('book.urls',namespace='book'))
  ```

  命名空间表示，凡是book.urls中定义的路由，均属于namespace指明的book名下

  命名空间的作用：避免不同应用中的路由使用了相同名字发生冲突，使用命名空间区别开

  2) 在定义普通路由时，可以使用name参数指明路由的名字: 如

  ```
  urlpatterns = [
      url(r'^$',index),
      # 匹配书籍列表信息的URL,调用对应的bookList视图
      url(r'^booklist/$',bookList,name='index'),
      url(r'^testproject/$',views.testproject,name='test'),
  ]
  ```

* 2. reverse反解析

  使用reverse函数，可以根据路由名称，返回具体的路径，如：

  ```
  from django.core.urlresolvers import reverse
  # 或
  from django.urls import reverse
  
  def testproject(request):
  
      return HttpResponse("OK")
  
  # 定义视图：提供书籍列表信息
  def bookList(request):
  
      url = reverse('book:test')
      print(url)
      return HttpResponse('index')
  ```

  - 对于未指明namespace的，reverse(路由name)
  - 对于指明namespace的，reverse(命名空间namespace:路由name)

### HttpRequest对象

利用HTTP协议向服务器传参的四种途径

​	提取URL的特定部分，如/weather/beijing/2018，可以在服务器端的路由中用正则表达式截取；

​	查询字符串（query string)，形如key1=value1&key2=value2；

​	请求体（body）中发送的数据，比如表单数据、json、xml；

​	在http报文的头（header）中。

- URL路径参数

  如果想从URL中获取值，需要在正则表达式中使用分组

  获取值分为两种方式

  * 位置参数

    位置参数不能错

  * 关键字参数

    参数的位置可以变，跟关键字保持一致即可

  **位置参数**

  应用中urls.py

  ```
   url(r'^(\d+)/(\d+)/$', views.index),
  ```

  视图中的函数: 参数的位置不能错

  ```
  def index(request, value1, value2):
        # 构造上下文
        context = {'v1':value1, 'v2':value2}
        return render(request, 'Book/index.html', context)
  ```

  **关键字参数**

  应用中的urls.py

  ​	其中?P\<value1>部分表示为这个参数定义的名称为values

  ​	可以是其他名称，起名要做到见名知意

  ```
  url(r'^(?P<value1>\d+)/(?P<value2>\d+)/$', views.index),
  ```

  视图中的函数：参数的位置可以变，跟关键字保持一致即可

  ```
  def index(request, value2, value1):      # 构造上下文      context = {'v1':value1, 'v2':value2}      return render(request, 'Book/index.html', context)
  ```

- Django中的QueryDict对象

  HttpRequest对象的属性GET、POST都是QueryDict类型的对象

  与python字典不同，QueryDict类型的对象用来处理同一个键带有多个值的情况

  * 方法get()：根据键获取值

    如果一个键同时拥有多个值将获取最后一个值

    如果键不存在则返回None值，可以设置默认值进行后续处理

    ```
    get('键',默认值)
    ```

  * 方法getlist()：根据键获取值，值以列表返回，可以获取指定键的所有值

    如果键不存在则返回空列表[]，可以设置默认值进行后续处理

    ```
    getlist('键', 默认值)
    ```

- 查询字符串Query String

  获取请求路径中的查询字符串参数（形如?k1=v1&k2=v2），可以通过request.GET属性获取，返回QueryDict对象。

  ```python
  # /get/?a=1&b=2&a=3
  
  def get(request):
  	a = request.GET.get('a')
  	b = request.GET.get('b')
  	alist = request.GET.getlist('a')
  	print(a) # 3
      print(b) # 2
      print(alist) # ['1', '3']
      return HttpResponse('ok')
  ```

  **重要：查询字符串不区分请求方式，即假使客户端进行POST方式的请求，依然可以通过request.GET获取请求中的查询字符串数据。**

- 请求体

  请求体数据格式不固定，可以是表单类型字符串，可以是JSON字符串，可以是XML字符串，应区别对待。

  可以发送请求体数据的请求方式有**POST**、**PUT**、**PATCH**、**DELETE**。

  **Django默认开启了CSRF防护**，会对上述请求方式进行CSRF防护验证，在测试时可以关闭CSRF防护机制，方法为在settings.py文件中注释掉CSRF中间件，

  * 表单类型 Form data

    前端发送的表单类型的请求体数据，可以通过request.POST属性获取，返回QueryDict对象。

    ```
    def post(request):
        a = request.POST.get('a')
        b = request.POST.get('b')
        alist = request.POST.getlist('a')
        print(a)
        print(b)
        print(alist)
        return HttpResponse('OK')
    ```

  * 非表单类型 Non-Form Data

    非表单类型的请求体数据，Django无法自动解析，可以通过**request.body**属性获取最原始的请求体数据，自己按照请求体格式（JSON、XML等）进行解析。**request.body返回bytes类型。**

    例如：要获取请求体中的如下JSON数据

    ```
    {"a":1, "b":2}
    ```

    可以进行如下操作

    ```python
    import json
    
    def post_json(request):
        json_str = request.body
        json_str = json_str.decode()
        req_data = json.loads(json_str)
        print(req_data['a'])
        print(req_data['b'])
        
        return HttpResponse('OK')
    ```

- 请求头

  可以通过**request.META**属性获取请求头headers中的数据，**request.META为字典类型**。

  常见的请求头如：

  - `CONTENT_LENGTH`– The length of the request body (as a string).
  - `CONTENT_TYPE`– The MIME type of the request body.
  - `HTTP_ACCEPT`– Acceptable content types for the response.
  - `HTTP_ACCEPT_ENCODING`– Acceptable encodings for the response.
  - `HTTP_ACCEPT_LANGUAGE`– Acceptable languages for the response.
  - `HTTP_HOST`– The HTTP Host header sent by the client.
  - `HTTP_REFERER`– The referring page, if any.
  - `HTTP_USER_AGENT`– The client’s user-agent string.
  - `QUERY_STRING`– The query string, as a single (unparsed) string.
  - `REMOTE_ADDR`– The IP address of the client.
  - `REMOTE_HOST`– The hostname of the client.
  - `REMOTE_USER`– The user authenticated by the Web server, if any.
  - `REQUEST_METHOD`– A string such as`"GET"`or`"POST"`.
  - `SERVER_NAME`– The hostname of the server.
  - `SERVER_PORT`– The port of the server (as a string).

  具体使用如下:

  ```python
  def get_headers(request):
      print(request.META['CONTENT_TYPE'])
      return HttpResponse('OK')
  ```

- 其他常用的HttpRequest对象属性

  - **method**：一个字符串，表示请求使用的HTTP方法，常用值包括：'GET'、'POST'。
  - **user：请求的用户对象。**
  - path：一个字符串，表示请求的页面的完整路径，不包含域名和参数部分。
  - encoding：一个字符串，表示提交的数据的编码方式。
    - 如果为None则表示使用浏览器的默认设置，一般为utf-8。
    - 这个属性是可写的，可以通过修改它来修改访问表单数据使用的编码，接下来对属性的任何访问将使用新的encoding值。
  - FILES：一个类似于字典的对象，包含所有的上传文件。

### HttpResponse对象

视图在接收请求并处理后，必须返回HttpResponse对象或子对象。HttpRequest对象由Django创建，HttpResponse对象由开发人员创建。

* HttpResponse

可以使用**django.http.HttpResponse**来构造响应对象。

```
HttpResponse(content=响应体, content_type=响应体数据类型, status=状态码)
```

也可通过HttpResponse对象属性来设置响应体、响应体数据类型、状态码：

​	content: 表示返回的内容

​	status_code: 返回的HTTP响应状态码

响应头可以直接将HttpResponse对象当做字典进行响应头键值对的设置：

```
response = HttpResponse()
responsep['itcast'] = 'Python'
```

示例:

```python
from django.http import HttpResponse

def response(request):
    return HttpResponse('itcast python', status=400)
    或者
    response = HttpResponse('itcast python')
    response.status_code = 400
    response['itcast'] = 'Python'
    return response
```

* HttpResponse子类

Django提供了一系列HttpResponse的子类，可以快速设置状态码

- HttpResponseRedirect 301
  - HttpResponsePermanentRedirect 302
  - HttpResponseNotModified 304
  - HttpResponseBadRequest 400
  - HttpResponseNotFound 404
  - HttpResponseForbidden 403
  - HttpResponseNotAllowed 405
  - HttpResponseGone 410
  - HttpResponseServerError 500

* JsonResponse

若要返回json数据，可以使用JsonResponse来构造响应对象，作用：

​	帮助我们将数据转换为json字符串

​	设置响应头**Content-Type**为**application/json**

```python
from django.http import JsonResponse

def response(request):
    return JsonResponse({'city': 'beijing', 'subject': 'python'})
```

* redirect重定向

```python
from django.shortcuts import redirect

def response(request):
    return redirect('/get_header')
```

### 状态保持

浏览器请求服务器是无状态的

无状态：指一次用户请求时，浏览器，服务器无法知道之前这个用户做过什么，每次请求都是一次新的请求

无状态的原因：浏览器与服务器是使用Socket套接字进行通信的，服务器将请求结果返回给浏览器之后，会关闭当前的Socket连接，而且服务器也会在处理页面完毕之后销毁页面对象。

有时需要保持下来用户浏览的状态，比如用户是否登录过，浏览过哪些商品等

实现状态保持主要有两种状态：

​	在客户端存储信息使用Cookie

​	在服务端存储信息使用Session

* Cookie

  Cookie是存储在浏览器中的一段纯文本信息，建议不要存储敏感信息如密码，因为电脑上的浏览器可能被其它人使用。

  **Cookie的特点**

  Cookie以键值对的格式进行信息的存储

  Cookie基于域名安全，不同域名的Cookie是不能互相访问的，如访问itcast.cn时向浏览器中写入Cookie信息，使用同一浏览器baidu.com时，无法访问到itcast.cn写的Cookie信息

  当浏览器请求某网站时，会将浏览器存储的跟网站相关的所有Cookie信息提交给网站服务器

  * 1. 设置Cookie

  可以通过HttpResponse对象中的set_cookie方法来设置cookie

  ```
  HttpResponse.set_cookie(cookie名, value=cookie值, max_age=cookie有效期)
  ```

  max_age单位为秒，默认为None，如果是临时cookie，可将max_age设置为None

  示例:

  ```python
  def cookie(request):
      response = HttpResponse('ok')
      response.set_cookie('itcast1','python1') # 临时cookie
      response.set_cookie('itcast2','python2',max_age=3600) # 有效期一小时
      return response
  ```

  * 2. 读取Cookie

  可以通过HttpRequest对象的COOKIES属性来读取本次请求携带的cookie值，request.COOKIES为字典类型

  ```python
  def cookie(request):
      cookie1 = request.COOKIES.get('itcast1')
      print(cookie1)
      return HttpResponse('ok')
  ```

  * 3. 删除Cookie

  可以通过HttpResponse对象中的delete_cookie方法来删除

  ```
  response.delete_cookie('itcast2')
  ```

* Session

  1. 启用Session

     Django项目默认启用Session，如需禁用session，将session中间件注释掉即可

  2. 存储方式

     在setting.py文件中，可以设置session数据的存储方式，可以保存在数据库、本地缓存等

     **2.1 数据库**

     存储在数据库中，如下设置可以写，也可以不写，这是默认存储方式

     ```
     SESSION_ENGINE = 'django.contrib.sessions.backends.db'
     ```

     如果存储在数据库中，需要在项INSTALLED_APPS中安装Session应用

     由表结构可知，操作Session包括三个数据: 键，值，过期时间

     **2.2 本地缓存**

     存储在本地内存中，如果丢失则不能找回，比数据库的方式读写更快

     ```
     SESSION_ENGINE='django.contrib.sessions.backends.cache'
     ```

     **2.3 混合存储**

     优先从本机内存中存取，如果没用则从数据库中存取

     ```
     SESSION_ENGINE='django.contrib.sessions.backends.cache_db'
     ```

     **2.4 Redis**

     在redis中保存session，需要引入第三方扩展，可以使用django-redis来解决

     1）安装配置

     ```
     pip install django-redis
     ```

     2) 配置

     在setting.py文件中做如下设置

     ```
     CACHES={
     	'default':{
     		'BACKEND': 'django_redis.cache.RedisCache',
             'LOCATION': 'redis://127.0.0.1:6379/1',
             'OPTIONS': {
                	'CLIENT_CLASS': 'django_redis.client.DefaultClient',
     	}
     }
     SESSION_ENGINE = 'django.contrib.session.backends.cache'
     SESSION_CACHE_ALIAS = 'default'
     ```

  3. Session操作

  通过HttpRequest对象的session属性进行会话的读写操作

  1）以键值对的格式写session

  ```
  request.session['键'] = 值
  ```

  2）根据键读值

  ```
  request.session.get('键', 默认值)
  ```

  3）清除所有的session,在存储中删除值的部分

  ```
  request.session.clear()
  ```

  4）清除所有的session,在存储中删除session的整条数据

  ```
  request.session.flush()
  ```

  5）删除session中的指定键和值，在存储中只删除某个键对应的值

  ```
  del request.session['键']
  ```

  6）设置session的有效期

  ```
  request.session.set_expiry(value)
  ```

  - 如果value是一个整数，session将在value秒没有活动后过期。
  - 如果value为0，那么用户session的Cookie将在用户的浏览器关闭时过期。
  - 如果value为None，那么session有效期将采用系统默认值， **默认为两周**，可以通过在settings.py中设置**SESSION_COOKIE_AGE**来设置全局默认值。

### 类视图和中间件

* 类视图

注册视图处理get和post请求

以函数的方式定义的视图称为**函数视图**，函数视图便于理解，但是遇到一个视图对应的路径提供了多种不同HTTP请求方式的支持时，便需要在一个函数中编写不同的业务逻辑，代码可读性与复用性都不佳

```python
def register(request):    # 处理注册        # 获取请求方式, 判断GET/POST请求    if request.method = 'GET':        # 处理GET请求, 返回注册页面        return render(request, 'register.html')    else:        # 处理POST请求, 实现注册逻辑        return HttpResponse('这里实现注册逻辑')	
```

​	**类视图的使用**

​	在Django中也可以使用类定义一个视图，称为类视图

​	使用类视图可以将视图对应的不同请求方式以类中不同方法来区别定义

```python
from django.views.generic import Views

class RegisterView(View):
    """类视图: 处理注册"""
    
    def get(self, request):
        """处理GET请求, 返回注册页面"""
        return render(request,'registerview.html')
    
    def post(self, request):
        """处理POST请求, 实现注册逻辑"""
        return HttpResponse('这里实现注册逻辑')
```

类视图的好处:

​	**代码可读性高**

​	**类视图相对于函数视图有更高的复用性**，如果其他地方需要用到某个类视图的某个特定逻辑，直接继承该类视图即可。

定义类视图需要继承自Django提供的父类View，可使用from django.views.generic import View，或者from django.views.generic.base import View导入，

配置路由时，使用类视图的as_view()方法来添加	

```python
urlpatterns = [
    # 视图函数：注册
    # url(r'^register/$', views.register,name='register'),
    # 类视图:注册
    url(r'^reguster/$', views.RegisterView.as_view(), name='register')
]
```

​	**类视图的原理**

```python
@classonlymethod
def as_view(cls, **initkwargs):
    """
    Main entry point for a request-response process.
    """
    ...省略代码...

    def view(request, *args, **kwargs):
        self = cls(**initkwargs)
        if hasattr(self, 'get') and not hasattr(self, 'head'):
            self.head = self.get
        self.request = request
        self.args = args
        self.kwargs = kwargs
        # 调用dispatch方法，按照不同请求方式调用不同请求方法
        return self.dispatch(request, *args, **kwargs)

    ...省略代码...

    # 返回真正的函数视图
    return view


def dispatch(self, request, *args, **kwargs):
    # Try to dispatch to the right method; if a method doesn't exist,
    # defer to the error handler. Also defer to the error handler if the
    # request method isn't on the approved list.
    if request.method.lower() in self.http_method_names:
        handler = getattr(self, request.method.lower(), self.http_method_not_allowed)
    else:
        handler = self.http_method_not_allowed
    =return handler(request, *args, **kwargs)
```

​	**类视图的多继承重新dispatch**

```python
class CenterView(View):

    def get(self,request):
        return HttpResponse("OK")

    def post(self,request):
        return HttpResponse("OK")
    
# 使用面向对象多继承的特性
class CenterView(LoginRequireMixin,View):

    def get(self,request):
        return HttpResponse("OK")

    def post(self,request):
        return HttpResponse("OK")
```

* 中间件

Django中的中间件是一个轻量级，底层的插件系统，可以介入Django的请求和响应处理过程，修改Django的输入输出，中间件的设计为开发者提供了一种无侵入式的开发方式，增强了Django框架的健壮性

**中间件的定义方式**

定义一个中间件工厂函数，然后返回一个可以被调用的中间件，

中间件工厂函数需要接收一个可以调用get_response对象。

返回的中间件也是一个可以被调用的对象，并且像视图一样需要接收一个request对象参数，返回一个response对象

```python
def simple_middleware(get_request):
    # 此处编写的代码仅在Django第一次配置和初始化的时候执行一次
    
        def middleware(request):
            # 此处编写的代码会在每个请求处理视图前被调用
            
            response = get_request(request)
            
            # 此处编写的代码会在每个请求视图后被调用
       		return response
     	return middleware
```

例如:

```python
def my_middleware(get_response):
    print('init 被调用')
    def middleware(request):
        print('before request 被调用')
        response = get_response(request)
        print('after response 被调用')
        return response
    return middleware
```

**定义好中间件后，需要在settings.py 文件中添加注册中间件**

```python
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    # 'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
    'book.middleware.my_middleware',  # 添加中间件
]
```

定义一个视图进行测试：

```python
def middleware(request):
    print('view 视图被调用')
    return HttpResponse('OK')
```

**注意：Django运行在调试模式下，中间件init部分有可能被调用两次。**

**多个中间件的执行顺序**

* 在请求视图被处理前，中间件由上至下依次执行
* 在请求视图被处理后，中间件由下至上依次执行

示例:

```python
# 定义两个中间件
def my_middleware(get_response):
   	print('init被调用')
    def middleware(request):
        print('before request被调用')
        response = get_response(request)
        print('after request 被调用')
        
        return response
    return middleware

def my_middleware2(get_response):
    print('init2 被调用')
    def middleware(request):
        print('before request 2 被调用')
        response = get_response(request)
        print('after response 2 被调用')
        return response
    return middleware

# 注册添加两个中间件
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    # 'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
    'users.middleware.my_middleware',  # 添加
    'users.middleware.my_middleware2',  # 添加
]

# 执行结果
init2 被调用
init 被调用
before request 被调用
before request 2 被调用
view 视图被调用
after response 2 被调用
after response 被调用
```

## 模板			

### Django使用自带模板

在工程中创建模板目录templates

在setting.py配置文件中修改TEMPLATES配置项的DIRS值

```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, 'templates')],  # 此处修改
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

在templates目录中新建一个模板文件，如index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
{{ city }}
</body>
</html>
```

调用模板分为两步骤：

1. 找到模板loader.get_templates(模板文件在模板目录中的相对路径) -> 返回模板对象
2. 渲染模板  模板对象.render(context=None, request=None) -> 返回渲染后的html文本字符串 context为模板变量字典，默认值为None request为请求对象，默认值为None

```python
from django.http import HttpResponse
from django.template import loader

def index(request):
    # 1. 获取模板
    template = loader.get_templates('index.html')
    
    context = {'city':'北京'}
    # 2. 渲染模板
    return HttpResoponse(templates.render(context))
```

​	Django提供了一个函数render可以简写上述代码

render(request对象，模板文件路径，模板数据字典)

```python
from django.shortcuts import render

def index(request):
    context = {'city':'北京'}
```

```python
def index(request):
    context = {
        'city': '北京',
        'adict':{
            'name':'西游记',
            'author': '吴承恩'
        }
        'alist':[1, 2, 3, 4, 5]
    }
    
    return render(request, 'index.html', context)
```

* 过滤器

​	使用管道符号来应用过滤器，用于进行计算，转换操作，可以使用在变量，标签中

​	如果过滤器需要参数，则使用冒号:传递参数

```
变量|过滤器: 参数
```

列举如下:

​	safe，禁用转义，告诉模块这个变量是安全的，可以解释执行

​	length，长度，然后字符串包含字符的个数，或列表，元组，字典的元素个数

​	default，默认值，如果变量不存在则返回默认值

```
data|default:'默认值'
```

​	date，日期，用于对日期类型的值进行字符串格式化，常用的格式化字符串如下：

​		Y表示年，格式为4位，y表示两位的年

​		m表示月，格式为01,02,12等

​		d表示日，格式为01，02等

​		H表示时，24进制，h表示12进制的时

​		i表示分，为0-59

​		s表示秒，为0-59

```
value|date:"Y年m月j日 H时i分s秒"
```

### Djago使用Jinja2模板

由于django默认模板引擎功能不齐全,速度慢，所以我们也可以在Django中使用jinja2, jinja2宣称比django默认模板引擎快10-20倍。

Django主流的第三方APP基本上也都同时支持Django默认模板及jinja2，所以要用jinja2也不会有多少障碍。

**安装jinja2**

```python
pip install jinja2
```

**Django配置jinja2**

1. 在项目文件中创建jinja2_env.py文件

```python
from jinja2 import Environment

def environment(**options):
    env = Environment(**options)
    
    return env
```

2. 在setting.py

```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.jinja2.Jinja2',#修改1
        'DIRS': [os.path.join(BASE_DIR, 'templates')],
        'APP_DIRS':True,
        'OPTIONS':{
            'environment': 'jinja2_env.environment',# 修改2
            'context_processors':[
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

**jinja2模板的使用绝大数和Django自带模板一样**

**jinja2自定义过滤器**

在jinja2_env.py文件中自定义过滤器

```python
from jinja2 import Environment

def environment(**options):
    env = Environment(**options)

    # 2.将自定义的过滤器添加到 环境中
    env.filters['do_listreverse'] = do_listreverse

    return env

# 1.自定义过滤器
def do_listreverse(li):
    if li == "B":
        return "哈哈"
```

### CSRF

CSRF译为跨站请求伪造

CSRF指攻击者盗用了你的身份，以你的名义发送恶意请求

​	包括：以你的名义发送邮件，发消息，盗取你的账户，甚至于购买商品，虚拟货币转账......

造成的问题：个人隐私泄露以及财产安全

**CSRF攻击示意图**

客户端访问服务器时没有同服务器做安全验证

**防止CSRF攻击**

1. 在客户端向后端请求界面数据的时候，后端会往响应中的 cookie 中设置 csrf_token 的值
2. 在 Form 表单中添加一个隐藏的的字段，值也是 csrf_token
3. 在用户点击提交的时候，会带上这两个值向后台发起请求
4. 后端接受到请求，以会以下几件事件：
   1. 从cookie中取出csrf_token
   2. 从表单数据中取出来隐藏的csrf_token的值
   3. 进行对比
5. 如果比较之后两值一样，那么代表是正常的请求，如果没取到或者比较不一样，代表不是正常的请求，不执行下一步操作
