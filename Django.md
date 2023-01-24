# 0Django框架

+ python三大主流web框架
  + django
    + 特点：大而全，自带功能十分的多
    + 缺点：过于笨重。
  + flask：
    + 特点：小而精，自带功能特别少，但是第三方模块比较多。
    + 缺点：比较依赖第三方的开发者。
  + tornado
    + 特点：异步非阻塞，支持高并发，甚至可以开发游戏服务器
    + 缺点：我不会。

### 注意事项

```python
# 如何然你的计算机能够正常的启动django项目
1.计算机的名称不能有中文
2.一个pycharm窗口只开一个项目
3.项目里面所有的文件尽量不要出现中文
4.python解释器尽量使用3.4-3.6之间的版本
5.如果项目报错那就去源码把逗号删掉
# django版本问题
pip install django==1.11.11
```



## Django基本操作

### 命令行操作

+ 创建django项目（django-admin startproject 项目名）
  + django-admin startproject web_server
+ 启动django项目（python manage.py runserver），这条指令后面可以跟ip和端口。如果不跟就默认是127.0.0.1：8000
  + python manage.py runserver
  + 注意在启动manage.py之前需要去到他的文件夹下面去
  + 使用cd web_server，可以使用ls查看文件夹下面的文件。
+ 创建应用（python manage.py startapp 名称）
  + python manage.py startapp app01
  + 创建完应用以后需要去配置文件中注册


```python
# 在settings文件中的INSTALLED_APPS中写"app01.apps.App01Config"，也可以简写app01
```



+ 应用：django框架是一款专门开发app的web框架
+ app名称应该见名知意

+ 修改端口号以及创建server

使用pycharm创建django项目

+ newproject选择左侧第二个django

### django项目主要文件介绍

+ web_server文件夹
  + wen_server文件夹
    + settings.py(项目配置文件)
    + urls.py（是路由与视图函数对应关系）
    + wsgi.py (wsgiref模块)
  + manage.py (是django的入口文件)
  + db.sqlite3  （django自带的sqlite3数据库，小型数据库功能不是很多，还有很多bug）
  + app01文件夹
    + admin.py(django的后台管理)
    + apps.py  (注册使用)
    + migrations文件夹 （数据库迁移记录）
    + models.py(数据库相关的 模型类（orm）)
    + tests.py(测试文件)
    + views.py（是一个 试图函数）

### 命令行和pycharm创建的区别

+ 使用命令行创建不会自动成templates文件夹，需要自己手动创建，但是pycharm会自动帮你创建并且还会自动在配置文件中配置对应的路径。
+ 使用命令行创建出来的文件里面没有自动配置路径，使用pycharm创建出来的文件会自动创建路径。

```python
# 命令行创建出来的文件
'DIRS': [],
# pycharm创建出来的文件
'DIRS': [BASE_DIR, '/templates']
```

### django小白必会三板斧

```python
"""
HttpResponse:使用来返回字符串
    # return HttpResponse("你好，我是django妹纸")
    

render：返回html页面
	# return render(request, "01.html")

redirect：
	# return redirect("https://www.bilibili.com/")
"""
```

### 静态文件配置

```python
"""
我们将html文件默认都放在templates文件夹下面
我们将网站使用的静态文件都默认放在static文件夹下面

静态文件
	前端已经写好了的，能过直接使用的文件。
	网站写好的js文件、css文件、网站使用到的图片第三方前端框架等等。
"""
# django默认是不会自动帮你创建static文件夹，需要自己手动创建
"""
一般情况下我们会在static文件夹下面还会进一步的划分处理
static
	js
	css
	img
	其他第三方文件
"""
"""
在浏览器中输入url能够看到 对应的资源是因为后端提前开设了该资源的接口
反过来说如果后端没有开设这个接口，那么就会访问不到资源
静态文件如果没有配置，也会访问不到。
"""

# django项目问题
"""
当你写django项目的时候可能会出现后端修改代码，但是前端页面没变化的情况
	1.你开了好几个django项目，一直再跑的是第一个django项目
	这个是因为端口号占用
	
	2.浏览器缓存问题
		settings
			network
				disable cache勾选上，这样就会清除缓存
"""

# 静态文件配置
    STATIC_URL = '/static/'   # 类似于访问静态文件的令牌
    """
    如果想要访问静态文件就需要以static开头
    """
    # 静态文件配置
    STATICFILES_DIRS = [
        os.path.join(BASE_DIR, 'static')
    ]
    """静态文件查找顺序是从上往下查找，如果有相同的文件可能会出现替换的情况，静态文件可以配置多个"""
    
# 动态获取静态文件令牌
{% load static %}  # 将static模块导进来
    <link rel="stylesheet" href="{% static 'bootstrap-3.4.1-dist/css/bootstrap.min.css' %}">
    
"""
from表单action参数
	不写就是默认向当前的url提交数据
	全写就会向写的地址提交参数
	只写后缀
"""

# 在前期我们使用django提交post请求的时候，需要去配置文件中注释掉一行代码
	"""
	MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    # 'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]
	"""
```



### request方法

```python
# request.method获取当前请求方式
# request.POST 获取用户提交的post数据（不包含文件）
    """
    request.POST.get() 会获取列表里面最后一个元素
    request.POST.getlist()会将一个列表完整的拿出来
    """


# request.GET获取用户提交的get数据
    """
    request.GET.get()
    request.GET.getlist()
    和post请求的用法一模一样
    """
```



```python
def login(request):
    """
    get请求和post请求应该有不同的处理机制
    :param request:
    :return:
    """
    print(request.method)   # 获取当前的请求方式，返回的是全大写的字符串形式的数据
    return render(request, 'login.html')


def login(request):
    if request.method =="POST":
        return HttpResponse("收到了 宝贝")
    elif request.method =="GET":
        return render(request, "login.html")

def login(request):
    if request.method == "POST":
        return HttpResponse("收到了 宝贝")
    return render(reuqest, "login.html")
```

### request method

```python
request.method
request.POST
request.GET
request.FILES

request.path  # only get url
request.path_info # only get url
request.get_full_path()  # can get url and url behind parameter(参数)
request.body # 获取原生的浏览器发过来的数据(一般是json数据)
```

### django连接数据库（mysql）

```python
# django默认是连接sqlite3
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}

# django连接mysql数据库
1.先要配置文件中的配置
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'web_serve',
        'USER': 'root',
        'PASSWORD': '123456',
        'HOST': '127.0.0.1',
        'PORT': 3306,
        'CHARSET': 'utf8'
    }
}
2.代码声明
	"""
	django默认的使用mysqldb连接mysql数据库
    但是，mysqldb的兼容性不好，所以需要手动改成使用pymysql连接
    所以我们需要告诉django不要使用mysqldb来连接数据库们要改成pymsql来连接数据库
	"""
    # 在项目名下面的init或者任意名下的init文件中书写以下代码都可以。
    import pymysql
	pymysql.install_as_MySQLdb()
    
"""
如果使用pycharm连接不了sqllite3，那么你需要下载sqllite3的驱动。
"""
```



### djangoROM

```python
"""
ORM对象关系映射
作用：是让不会sql语句的小白也能使用python代码快捷的操作数据库
不足之处：封装程度太高，有时候sql语句的效率太低，需要自己写sql语句
	类			 		 表
	对象					记录
	对象属性			   记录某个字段对应的值

"""
# 先去models.py中写一个类
class User(models.Model):
    # id int primary_key auto_increment
    id = models.AutoField(primary_key=True)
    # username varchar(32)
    username = models.CharField(max_length=32)
    # password int
    password = models.IntegerField()
    
#数据库迁移命令
	# 将操作记录记录在migrations文件夹中
    python manage.py makemigrations
    # 将操作同步到数据库中
    python manage.py migrate
    # 只要修改了models.py中和数据库相关的代码就需要重新执行上诉两条命令。
```

![image-20221112122239495](C:\Users\阿良\AppData\Roaming\Typora\typora-user-images\image-20221112122239495.png)



### 利用ROM实现数据的增删改查

#### 字段的增删改查

```python
# 字段的增加
	"""
	1.可以在终端内直接给出默认值
	2.null=True字段可以为空
    info = models.CharField(max_length=50,verbose_name='个人简介', null=True)
    3.default="默认值"
    hobby = models.CharField(max_length=20,verbose_name="兴趣爱好",default="study") 
	"""
```

#### 数据的增删改查

```python
# 查找数据
username = request.POST.get("username")
password = request.POST.get("password")
# print(password, username)
# select * from app01_user where username="egen";
info = models.User.objects.filter(username=username)
"""
filter里面可以携带多个参数，如果写多个参数那么参数与参数之间就会默认以and的形式连接
可以把filter看成是sql语句中的where。
"""
# <QuerySet [<User: User object>]> 拿到的是列表套数据对象
print(info) # 可以索引取值，但是不支持负数索引取值
info.first() # 取里面第一个元素 
print(info[0].username)
print(info[0].password)
    
# 增加数据
第一种方式
	data = models.User.objects.create(username=username, password=password)
第二种方式
	data = models.User(username=username, password=password)
    data.save()  # 保存数据
    
```

### 数据的删改查

```python
# 将数据库的数据展示到前端，然后给每一个数据两个按钮，一个编辑，一个删除。
# 编辑功能
	# 点击编辑按钮，然后从后端发送编辑数据的请求。
    """
    如何告诉后端用户想要编辑那条数据？
    	将编辑按钮所在的那一行主键值发送给后端
    	利用url问号后面的携带参数的方式将主键值传给后端
    """
def user_lis(request):
    # 取出全部数据
    # user_list = models.User.objects.filter()
    # 取出全部数据
    user_list = models.User.objects.all()
    print(user_list)
    return render(request, "userlis.html", locals())


# models.User.objects.filter(id=user_id).update(username=username, password=password)
def edit_user(request):
    user_id = request.GET.get("user_id")
    if request.method == "GET":
        return render(request, "edit_user.html")
    # 方式1：
    username = request.POST.get("username")
    password = request.POST.get("password")
    models.User.objects.filter(id=user_id).update(username=username, password=password)
    """批量操作，速度比较快"""
    # 方式2：
    # data = models.User.objects.filter(id=user_id).first()
    # data.username = username
    # data.password = password
    # data.save()
    """速度比较慢，会将查询出来的数据全部改一遍"""
    return redirect("/userlis/")


# data = models.User.objects.filter(id=user_id).delete()
def remove(request):
    user_id = request.GET.get("user_id")
    data = models.User.objects.filter(id=user_id).delete()
    return redirect("/userlis/")

```

### djangoORM创建表关系

```python
"""
表和表之间的关系
一对一

多对一

多对多
"""
# 创建表关系 先将基表创建出来，然后在添加外键字段字段
class Book(models.Model):
    title = models.CharField(max_length=32)
    # max_digits=8表示最大长度8位，decimal_places=2表示小数点后面占两位。
    price = models.DecimalField(max_digits=8, decimal_places=2)
    # 表示将publish设置成为外键，和Publish表做关联，如果不写to_fields就会默认和主键字段做关联
    publish = models.ForeignKey(to="Publish")
    """如果字段对应的是foreign key 那么ORM会自动的在字段后面加上_id
    就算你在外键上面加上_id ORM依然会在后面加上_id。一对多的话外键字段建在多的一方"""
    # 图书和作者是多对多的关系，外键字段建在任意一方即可，但是推荐建在查询频率比较高的表
    authors = models.ManyToManyField(to="Author")
    """
    authors是一个虚拟字段，主要是告诉ORM书籍表和作者表的多对多的关系
    让ORM帮你创建第三张表
    """


class Publish(models.Model):
    name = models.CharField(max_length=32, verbose_name="出版社名称")
    addr = models.CharField(max_length=64, verbose_name="出版社地址")


class Author(models.Model):
    name = models.CharField(max_length=32, verbose_name="作者名字")
    age = models.IntegerField(verbose_name="作者年龄")
    # 一对一关系创建外键字段可以建在任意一方，但是推荐建立在查询频率比较高的表中
    author_detail = models.OneToOneField(to="AuthorDetail")
    """也会在后面给你加_id"""


class AuthorDetail(models.Model):
    addr = models.CharField(max_length=64, verbose_name="作者地址")
    phone = models.BigIntegerField()  # 直接写字符串类型也行

"""
在django1.X中外键都是级联更新级联删除的。
在django2.x和3.x中就需要设置级联更新和级联删除
on_delete=models.CASCADE
"""
```

### django生命周期请求流程图

```python
# 扩展知识点
"""
缓存数据库
	提前已经将你想要的 数据准备好了 来了就可以直接拿
	这样就可以提高效率和响应时间
"""
```

![image-20221115001150925](C:\Users\阿良\AppData\Roaming\Typora\typora-user-images\image-20221115001150925.png)

## 路由层

```python
# 路由匹配
url(r"test",views.test),
url(r"testadd",views.testadd),

"""
url第一个参数是正则表达式，
	只要第一个参数正则表达式能过匹配到内容，那么就会立刻停止往下匹配，
	直接执行对应的视图函数。
	
你在输入url的时候会自动的加上/，是因为django自动的帮你做了重新向，，匹配一次不行那就加上/在匹配一次
	
	取消自动加/
	APPEND_SLASH = False

"""
```

### 	无名分组和有名分组

```python
# 无名分组
url(r'^test/(\d+)/', views.test),

def test(request, aa):
    print(aa)
    return HttpResponse("test")
"""
无名分组就是将括号内正则表达式匹配到的位置参数当作位置参数传递给后面的视图函数
"""

# 有名分组
url(r'^testadd/(?P<year>\d+)/', views.testadd),

def testadd(request, year):
    print(year)
    return HttpResponse("testadd")
"""
又名分组就是将括号内正则表达式匹配到的内容当作关键字参数传给后面的视图函数
"""

"""
无名分组和有名分组能不能混合使用？
	不能混用，但是同一个分组可以使用N多次。
"""
```

### 反向解析

```python
"""
反向解析的本质：
	通过一些方法得到一个结果，该结果可以访问到对应的url从而触发视图函数的运行
"""
# 最简单的情况url第一个参数里面没有正则符号
url(r'^func/', views.func, name='ooo')
# 两种解析的情况
	"""
	前端
		{% url 'ooo'%}
		
	后端
		需要reverse方法
		from django.shortcuts import
		print(reverse('ooo'))
		别名不能冲突
		
	"""

# 后端
reverse("ooo", args(1,))
# 前端
{% url"ooo" 1 %}

```

### 路由分发

```python
"""
django的每一个应用都可以有自己的templates文件夹urls.py和static文件夹。
正是基于上诉的特点，django能够很好的做到分组开发（每个人只写自己的app）

当一个django项目里面的app特别的多的时候，总路由urls.py代码非常不好维护，这时候也可以利用路由分发来减轻总路由的压力。

利用路由分发以后，总路由不再干路由与视图的直接对应关系，而是做一个分发处理，识别当前的url是哪一个app下的，在分发给对应的app去做处理
"""

# 总路由
	from django.conf.urls import url, include
from django.contrib import admin
# from ..app02 import urls as app02_urls
# from ..app01 import urls as app01_urls


urlpatterns = [
    url(r'^admin/', admin.site.urls),

    # 路由分发
    # url('^app01/', include(app01_urls)),  # 只要url前缀是app01的都交给app01_urls处理
    # url('^app02/', include(app02_urls)),  # 只要url前缀是app01的都交给app01_urls处理
    # 路由分发终极写法
    url(r'^app01/', include('app01.urls')),
    url(r'^app02/', include('app02.urls')),
]

# 子路由
	from django.conf.urls import url
import views

urlpatterns = [

]

```



### 名称空间

```python
"""
当多个应用出现了相同的别名，在正常情况下，反向解析是不会自动识别前缀的，因此在不同的应用里面出现相同的名字时，逆向解析是无法解析出相对应的url的。

在面对这种情况时，我们可是使用名称空间
"""
# namespace 创建名称空间
    url(r'^app01/', include('app01.urls', namespace='app01')),
    url(r'^app02/', include('app02.urls', namespace='app02')),
reverse('app02:reg')
{% url 'app02:reg' %}
# 一般情况下，多个app之间在取别名的时候，会加上app的前缀，
# 这样的话会保证app之间的别名不会起冲突。

```

### 伪静态页面

```python
"""
静态页面
	静态页面就是写死的万年不变的
伪静态
	将一个动态页面伪装成为静态页面
为什么要将一个动态页面伪装成为一个静态页面呢？
	伪装的目的在于增大本网站的seo查询力度
    并且增加搜索引擎搜索本网站的概率（如果网页是一个静态页面，那么搜索引擎就会优先把网页收藏起来，等用户访问的时候就可以优先将页面发送过去。）
	伪装静态页面只需要在url后面加上html后缀就行
	url(r'^index.html', views.index),		
	
搜索引擎本质上就是一个巨大的爬虫程序

总结：
	无论你怎么优化，怎么处理，都干不过RMB玩家

"""
```

### 虚拟环境

```python
"""
在正常开发中，我们会给每一个项目配备一个独有的开发环境，
在这个环境里面只有该项目使用的模块，其他的模块都不装

虚拟环境
	虚拟环境就类似于一个单独的python解释器环境（你每创建一个虚拟环境就类似于重新下载一个纯净的python解释器）
	但是虚拟环境不要创建太多，是需要消耗内存空间的。
	
拓展：
	每一个项目都需要用到很多模块，并且每一个模块的版本还不一样。
	那我们应该如何安装？
	在开发中我们会给每一个项目配备一个requirements.txt文件
	里面书写该项目需要使用到的模块及版本就可以使用命令一键安装。
	

"""
```

### django版本区别

```python
"""
django1版本路由层使用的是url方法
django2和django3版本使用的是path方法
	url()方法第一个参数是支持正则的
	path()方法第一个参数是不支持正则的，写什么就匹配什么。
	
	如果你不习惯使用path方法django给你提供了re_path方法。
	from django.urls import path, re_path
	想使用url方法就导入from django.conf.urls import url
	
    2版本和3版本里面的re_path就等价于url

虽然path不支持正则，但是他的内部支持五种转换器。
    str,匹配除了路径分隔符（/）之外的非空字符串，这是默认的形式
    int,匹配正整数，包含0。
    slug,匹配字母、数字以及横杠、下划线组成的字符串。
    uuid,匹配格式化的uuid，如 075194d3-6885-417e-a8a8-6c931e272f00。
    path,匹配任何非空字符串，包含了路径分隔符（/）（不能用？）
"""
# path除了有五个转换器之外还支持自定义转换器（了解）。
class MonthConverter:
    regex = '\d{2}' # 属性名必须是regex
    def to_python(self, value):
        return int(value) # 匹配的regex是两个数字，那么返回的结果也必须是两个数字
    
    def to_url(self, value):
        return value

from django.urls import path, register_converter
from app01.path_converts import MonthConverter

# 先注册转换器
register_converter(MonthConverter,'mon')
from app01 import views

urlpatterns = [
    
]

"""
在1版本里面外键都是级联跟新级联删除的，但是到了2版本和3版本中需要自己手动pei

"""
```

### 视图层

```python
"""
HttpResponse
	返回字符串类型
f
	返回html页面 并且在返回html页面之前可以给html页面传值
redirect
	重定向
"""
# 试图函数必须要返回一个HttpResponse对象
# render简单的内部原理

```

### jsonresponse对象

```python
# jsonresponse默认只能序列化字典，如果想要序列化其他数据类型就需要加safe参数

```

### form表单上传文件

```python
"""
form表单上传文件类型数据
	1.method必须指定成post
	2.enctype必须换成formdata
"""
request.POST # 不可以拿到文件数据只能拿到普通的键值对数据
request.FILES # 专门拿文件数据
request.FILES.get('file')  # get file object
```

### FBV与CBV

```python
# 视图函数既可以是函数也可以是类
# FBV
def func(request):
    return HttpResponse('index')

# CBV
    # 路由
    re_path(r'^login/', views.Login.as_view())

    # class类写法
    from django.views import View
    class Login(View):
        def get(self, request):
            return render(request, 'register.html')

        def post(self, request):
            return HttpResponse('post')

"""
FBV和CBV各有千秋
	CBV特点：
		能够根据请求的方式不同直接匹配到对应的方法执行
	内部到底怎么实现的？
	

"""
```

### CBV源码剖析

```python
```

### django模板语法

```python
"""
django模板语法可以传python里面的任何数据类型，甚至可以传递函数名，帮你加括号在后端调用函数，并且将函数的返回值展现到前端页面。但是django模板语法里面的函数不能传递参数。

django模板语法在传值给前端的时候，会自动的判断能不能加括号调用。如果加括号能够调用的话那么就会自动调用。并且把返回值返回给前端。
"""
# 过滤器语法
"""
过滤器就类似于模板语法内置的方法 内置方法
django里面有60多种过滤器

过滤器基本语法
	{{数据|过滤器:参数}}
	例如：
		s = "你有一个好朋友"
		{{s|length}}
"""
# 常见的过滤器

{{s|length}}
b = True
{{b|default:啥也不是}}
default如果b是True那么就取True，如果b是False那么就取啥也不是

# 文件大小
filesizeformat会自动的帮你计算文件的大小

# 日期格式化
import datetime
current_time = datetime.datetime.now()
{{current_time|date:'Y-m-d H:i:s'}}

# 列表切片操作
{{l|slice:'0:4:2'}}
# 切取字符串
{{ info:truncatechars:9 }} # 切取9个字符，包括后面的三个点
# 切取英文单词
{{info:truncatewords:9}} # 切取9个英文单词是不包含后面的三个点的。
# 移除特特定的字符
{{info|cut:' '}}  # 移除空字符
# 拼接操作
{{info|join:'$'}}  # 使用$将列表里面的字符串拼接起来
# add拼接操作（如果是数字和数字的拼接那么就会将两个数字相加，如果是字符串，那么就会将两个字符串拼接起来）
{{info|add:10}}
{{info|add:'你好'}}
# 前端转义
h1 = '<h1>我要转义</h1>'
{{h1|safe}}
# 后端转义传给前端
from django.utils.safestring import make_safe
res = make_safe(h1)
{{res}}
"""
在学了转义以后，你的前端代码不一定要写在前端页面里面，还可以将js代码写在后端里面。

"""

forloop参数
{% for foo in l %}   
    <p>{{ forloop }}</p> 
{% endfor %}
"""
{'parentloop': {}, 'counter0': 0, 'counter': 1, 'revcounter': 3, 'revcounter0': 2, 'first': True, 'last': False}

{'parentloop': {}, 'counter0': 1, 'counter': 2, 'revcounter': 2, 'revcounter0': 1, 'first': False, 'last': False}

{'parentloop': {}, 'counter0': 2, 'counter': 3, 'revcounter': 1, 'revcounter0': 0, 'first': False, 'last': True}
"""

if判断语句
b = False
{% if b %}
<p>boby</p>
{% elif True %}
    <p>old boby</p>
{% else %}
    <p>new boby</p>
{% endif %}

for循环和if判断的混合使用
{% for foo in l %}
    {% if forloop.first %}
        <p>this is my first</p>
    {% elif forloop.last %}
        <p>This is my last</p>
    {% else %}
        <p>{{ foo }}</p>
    {% endif %}
    {% empty %}
    <p>列表里面没有元素，无法循环</p>

{% endfor %}
```

### 自定义过滤器、标签、inclusion_tag

```python
"""
三步走：
	1.在应用下面创建一个文件夹，名字”必须“要叫templatetags
	2.在文件夹内可以创建任意名字的py文件
	3.在pu文件里面必须书写以下两句话(变量名必须是register不能更改)
	from django import template
	register = template.Library() 

"""
# 过滤器最多只能传递两个参数
# 自定义过滤器
@register.filter(name='sum')
def sum(m1, m2):
    return m1+m2
# 页面使用
{% load mytag %}
<p>{{n|sum:12}}<p/>

# 自定义标签（标签可以是任意多个） 标签彼此之间要空格隔开
@register.simple_tag(name='plus')
def index(a, b, c):
    return '%s-%s-%s'%(a, b, c)
{% load mytag %}
<p>{% plus 'jeason' 123 123 %}</p>

```

### 模板的继承

```python
"""
模板
{% extends 'home.html' %}

# 为区域取别名，
{% block content %}
   <div class="panel-body">
    <div class="jumbotron">
  <h1>Hello, world!</h1>
  <p>...</p>
  <p><a class="btn btn-primary btn-lg" href="#" role="button">Learn more</a></p>
</div>
  </div>
   {% endblock %}
  
# 在子页面上声明想要修改的区域
{% block content %}
<h1>注册页面</h1>
    <form action="">
    <p>username:<input type="text" class="form-control"></p>
    <p>password: <input type="password" class="form-control"></p>
    <p><input type="submit" class="btn btn-danger"></p>
    </form>
{% endblock %}

"""
# 一般来说模板页面至少要有三块可以被修改的区域
"""
1.css区域
2.js区域
3.html区域
这样写了以后，每一个子页面都可以有自己的css代码和html代码还有js代码
一般来说模板划定的越多那么模板的可拓展性也就越强，但是如果一个页面的划定的区域太多还不如自己写
"""
```

### 模板的导入

```python
{% include '模板.html' %}

```

### 测试脚本

```python
"""
当你只是想测试django中的某一个py文件的内容时 那么你可以不用书写前后端交互的形式，而是写一个测试脚本即可

脚本代码无论是写在应用下的tests.py中还是写在自己新建的py文件中都是可以的

注意所有的测试代码都需要写在测试环境下面
"""
# 测试环境的准备
import os
if __name__ == '__main__':
    os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'day64.settings')
    import django
    django.setup()
```

### 必知必会十三条

```python
# 使用频率由高到低排序
all()  # 查询所有数据
filter() # 带有过滤条件的查询
get()  # 直接拿数据对象，但是条件不存在的话直接报错
first()  # 拿到queryset里面的第一个元素
last()  # 拿到queryset对象里面的最后一个元素

values() # 可以指定要获取的字段，values的返回结果是列表套字典
models.User.objects.create(name='jason', password='123456', age=84, register_time='1942-5-11')

values_list()  # values_list返回的是列表套元组作用和values一样 
 ret = models.User.objects.values_list('name', 'age', 'password') 


distinct()  # 去重
"""
去重一定要是一摸一样的数据如果包含主键的话，那么就肯定不能去重。
"""
ret = models.User.objects.values_list('name', 'age', 'password').distinct()


order_by()  # 排序
models.User.objects.order_by("age") # 默认升序
models.User.objects.order_by("-age") # 降序

reverse()  # 反转，反转的前提是数据已经排序了
ret.order_by('-age').reverse()

count()  # 统计当前数据的个数
exclude() # 排除在外
exites()  # 判断数据是否存在
```

### 查看内部sql语句的方式

```python
# 方式1：（只能对queryset对象使用）
	ret.query  # 查看内部的sql语句

# 方式2：（通用对象）
LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,
    'handlers': {
        'console':{
            'level':'DEBUG',
            'class':'logging.StreamHandler',
        },
    },
    'loggers': {
        'django.db.backends': {
            'handlers': ['console'],
            'propagate': True,
            'level':'DEBUG',
        },
    }
}
# 将上面代码复制到settings文件中

```

### 神奇的双下划线(查询)

```python
# __gt大于
# 找到年龄大于三十五的人
rev = models.User.objects.filter(age__gt=35).first()
print(rev.name)

# __lt小于
# 找到年龄小于三十五的人
rec = models.User.objects.filter(age__lt=35)
for i in rec:
    print(i.name)
    
# __gte大于等于, __lte小于等于
# age__in=[18, 32], 查询年龄是18或者32的人
# age__range=[18, 40]查询年龄在18到40这个区间的人

# 模糊查询（模糊查询默认是区分大小写的）
# 查询名字中包含n这个字符的元素
models.User.objects.filter(name__contains='n')
# 忽略大小写
models.User.objects.filter(name_icontains='n')

# 按照某一个年份或者月份查询
models.User.objects.filter(create_time_year)

# 查询以什么开头或者以什么结尾
models.User.objects.filter(name_startswith='n')
models.User.objects.filter(name_en'd='n')
```

### 外键的增删改查

```python
"""一对多增删改"""
# 增加
	# 直接写实际的字段
    # models.Publish.objects.create(name='')
    # models.Book.objects.create(title='李德胜选集', price=1, publish_id=2)
    # 写虚拟字段
    # publish_boj = models.Publish.objects.filter(id=3).first()
    # models.Book.objects.create(title='稻上飞反动计', price=5000, publish=publish_boj)

# 删除
	models.Publish.objects.filter(pk=1).delete() # 级联删除
    
# 修改
	models.Book.objects.filter(pk=3).update(publish_id=2)
    publish_obj0 = models.Book.objects.filter(pk=3).first()
    models.Book.objects.filter(pk=3).update(publish=publish_obj0)

"""多对多增删改"""
	# 增加数据
    book_obj = models.Book.objects.filter(pk=2).first()
    print(book_obj.author)  # 查看到了第三张关系表
    book_obj.author.add(2)  # 给id为2的书籍绑定一个主键为2的作者
    # book_obj.author.add(3, 4)  # add可以添加好几个参数，也就是可以同时为同一本书添加几个作者
    author_obj = models.Author.objects.filter(pk=2).first()
    book_obj.author.add(author_obj)
    """
    add给第三张表添加数据
        括号内既可以传数据也可以传对象 并且都支持多个
    """
    # 删除数据
    book_obj.author.remove(2)
     """
    remove给第三张表删除数据
        括号内既可以传数据也可以传对象 并且都支持多个
    """
    # 修改数据
    book_obj.author.set([1, 2])  # set的参数必须是一个可迭代对象
    """
    set给第三张表更改数据
        括号内既可以传数据也可以传对象 并且都支持多个
    	如果更改就是将原数据删除再增加一条数据
 
    """

    # 清空
    # 在第三张表中清空某个书籍与作者的绑定关系
    book_obj.author.clear()
    """
    clear 里面不要放任何参数
    """
```

## 多表查询

```python
# 正向查询按字段，反向查询按表名小写 
"""
正反向概念
	正向：看外键字段在谁那（外键字段在书籍上面，那么由书籍对象查询作者表，那么就是正向）
	反向：由出版社查书那么就是反向字段
"""
# 子查询（基于对象的跨表查询）
	# 1.查询书籍主键为2的出版社名称
    book_obj = models.Book.objects.filter(pk=2).first()
    res = book_obj.publish
    print(res.name, res.addr)

    # 2.查询书籍主键为2的作者
    ret = book_obj.author.all()
    print(ret[0].author_name)
    # app01.Author.None 如果查询结果出现这个，那么可能不是你的ORM语句写错了而是需要在book_obj.author后面加上.all()
    # print(ret.name)

    # 3.查询作者李德胜的电话号码
    author_obj = models.Author.objects.filter(author_name='李德胜').first()
    rev = author_obj.author_detail
    print(rev.phone)
    
    # 4.查询出版社是得胜出版社出版的书
    publish_obj = models.Publish.objects.filter(name='德胜出版社').first()
    res = publish_obj.book_set.all()
    print(res[0].title)
    print(res[1].title)

    # 5.查询出作者李德胜写的书
    author_obj = models.Author.objects.filter(author_name='李德胜').first()
    rev = author_obj.book_set.all()
    print(rev[0].title)
	
"""
什么时候需要加.all()，什么时候不需要加.all()?
	当你查询的结果可能返回多个的时候，就需要使用.all()，
	如果是一个那么就可以直接拿到数据对象 

什么时候查询结果需要加_set.all()?(一对一查询不用加__set.all())
	当你的查询结果可以有多个的时候，就必须加__set.all()
	当你查询结果只有一个的时候就不需要加_set.all()
"""


# 连表查询（基于双下划线的跨表查询）
    # 6.查询李德胜的手机号
    # values就是表示跨到关联到author_detail的字段的表去查询数据
    res = models.Author.objects.filter(author_name='李德胜').values('author_detail__phone')
    print(res[0]['author_detail__phone'])

    # 7.查询书籍主键为2的出版社名称和书的名称
    rec = models.Book.objects.filter(pk=2).values('publish__name', 'publish__book__title', 'title')
    for i in range(2):
        print(rec[i]['publish__name'], rec[i]['publish__book__title'])
    print(rec)

	# 8.查询李德胜的手机号(不能使用.author)
    rex = models.AuthorDetail.objects.filter(author__author_name='李德胜').values('phone')
    print(rex)

```

### 分组聚合函数使用

```python
 """
 只要是跟数据库相关的模块，那基本上都在django.db.models里面
 如果没有那么应该就在django.db里面
 """
# 聚合查询（聚合查询通常情况下都是配合分组一起使用的）如果你不想在分组之后使用，那么就需要再加一个关键字aggregate

# 导入模块
from django.db.models import Max, Min, Sum, Count, Avg
rec = models.Book.objects.aggregate(Avg('price'))
print('平均价格:', rec)

"""
只要orm语句的出来的是queryset对象，
那么就可以无限的使用query封装的方法
"""

"""
如果我想按照指定的字段分组应该如何处理呢？
	models.Book.objects.values('price').annotate()
	这样就是通过price来分组
	如果annotate前面没有出现values就会通过models后面的表分组，如果有values那么就按照values里面的字段分组。

注意：
	如果在你的机器上面一分组就报错，那么就需要解除数据库的严格模式
"""
```

F和Q查询

```python
# F与Q查询
    # F查询
    """
    帮你直接获取到表中某个字段对应的数据
    """
    from django.db.models import F
    # 1.查询卖出数大于库存书的书籍
    # sell_obj = models.Book.objects.filter()
    reb = models.Book.objects.filter(repertory__lt=F('sell'))
    for i in range(3):
        print('book_name:', reb[i].title)

    # 2.将所有书的价格提升五十块
    # models.Book.objects.update(price=F('price')+50)

    # 3.将所有的书后面加上爆款两个字
    """
    注意：在操作字符类型的数据时，F是不能直接拼接字符串的
    """
    from django.db.models.functions import Concat
    from django.db.models import Value
    models.Book.objects.update(title=Concat(F('title'), Value('爆款')))
	"""
	models.Book.objects.update(title=F('title')+'爆款')
	如果直接像上面那样操作，所有的名称都会变成空白
	"""
    
    # Q查询（能够修改多个查询条件之间的关系）
    # 1.查询卖出数大于5000小于50000的书籍
    # models.Book.objects.update()
    from django.db.models import Q
    # rec = models.Book.objects.filter(Q(sell__gt=5000), Q(sell__lt=50000))
    # rec = models.Book.objects.filter(Q(sell__gt=5000) | Q(sell__lt=50000))
    # rec = models.Book.objects.filter(~Q(sell__gt=5000)|Q(sell__lt=50000))

    """
    使用,连接的还是and关系，使用|连接的就是or关系，使用~连接的就是非关系连接
    """
    print(rec[0].title)
    
    # Q的高阶用法，能将查询条件的左边也能变成字符串的形式,这样就能动态的指定查询的条件。
    q = Q()
    q.connector = "or"  # 不写的话就默认是and
    q.children.append(('sell__gt', 5000))
    q.children.append(('sell__gt', 50000))
    ret = models.Book.objects.filter(q)
    print(ret)
```

### django开启事务

```python
"""
事务：
	ACID：
		原子性：不可分割的最小单位
		一致性：和原子性的相辅相成的
		隔离性：事务之间互不干扰
		持久性：事务一旦确认永久生效
	事务回滚：
		rollback
	事务确认：
		commit
	
"""
    # 事务
    from django.db import transaction
    try:
        with transaction.atomic():
            # sql1
            # sql2
            # with代码内书写的所有orm操作属于同一个事务
            pass
    except Exception as e:
        print(e)
```

### 数据库设计三大范式

```python
# 数据库设计三大范式
数据库的设计范式是数据库设计所需要满足的规范，满足这些规范的数据库是简洁的、结构明晰的，同时，不会发生插入（insert）、删除（delete）和更新（update）操作异常。

一、第一范式（1NF）:列不可再分
1.每一列属性都是不可再分的属性值，确保每一列的原子性

2.两列的属性相近或相似或一样，尽量合并属性一样的列，确保不产生冗余数据

二、第二范式（2NF）属性完全依赖于主键
第二范式（2NF）是在第一范式（1NF）的基础上建立起来的，即满足第二范式（2NF）必须先满足第一范式（1NF）。第二范式（2NF）要求数据库表中的每个实例或行必须可以被惟一地区分。为实现区分通常需要为表加上一个列，以存储各个实例的惟一标识。这个惟一属性列被称为主键

三、3.第三范式（3NF）属性不依赖于其它非主属性    属性直接依赖于主键
数据不能存在传递关系，即每个属性都跟主键有直接关系而不是间接关系。像：a-->b-->c  属性之间含有这样的关系，是不符合第三范式的。
比如Student表（学号，姓名，年龄，性别，所在院校，院校地址，院校电话）
这样一个表结构，就存在上述关系。 学号--> 所在院校 --> (院校地址，院校电话)
这样的表结构，我们应该拆开来，如下。
   （学号，姓名，年龄，性别，所在院校）--（所在院校，院校地址，院校电话）
    
总结：三大范式只是一般设计数据库的基本理念，可以建立冗余较小、结构合理的数据库。如果有特殊情况，当然要特殊对待，数据库设计最重要的是看需求跟性能，需求>性能>表结构。所以不能一味的去追求范式建立数据库。
```



### ORM中常用的字段和参数

```python
AutoField
	主键字段 primary key=True
CharField  
	verbose_name，max_length
IntegerField
BigIntegerField

DecimalField
	max_digits=8
    decimal_places=2
EmailField  vachar(254)

DateField
DateTimeField
	auto_now:每次修改数据的时候都会自动更新当前时间
    auto_now_add：只在创建数据的时候记录创建的时间，后续就不会再改动。
BooleanField --是布尔值数据类型
该字段存储布尔值的时候是零和一的形式

TextField  --文本类型
	该字段可以用来存储大段文章内容（没有字数限制）

FileField --文件字段
	upload_to='/data' 给该字段一个文件对象，会自动保存到data文件下面。然后将文件路径保存到数据库中。
    
ImageField  --专门用来保存图片的

# django除了给你提供了很多字段类型以外，还支持你自定义字段

# 外键字段及参数
unique=True 设置唯一
	ForeignKey(unique=True)就相当于OneToOneField()
    
db_index
	如果设置db_index=True那么久意味着将此字段设置索引（索引是什么？复习）

to_field
	设置要关联的表的字段 默认不写就是关联主键
    
on_delete
	设置级联删除    
```

### 数据库查询优化

```python
only与defer
select_related与prefetch_related

"""
orm语句的特点：
	惰性查询：如果你只是书写了ORM语句，在后面根本没有用到该语句查询出来的参数，那么ORM会自动识别，直接不执行。
"""
# only
    # 使用only
    rec = models.Book.objects.only('title')  # 拿到只包含title的数据对象,如果想要拿到title字段里面的内容，就不用走数据库
    # 如果要拿到其他字段里面的内容就需要走数据库。
    for i in rec:
        print(i.title)

# defer
	res = models.Book.objects.defer('title')
    """
    defer和only刚好相反，查询括号内的数据就需要走数据库，但是查询非扩号内的数据就需要走数据库。
    """
    
# select_related和prefecth_related跨表操作有关
	ret = models.Book.objects.select_related('publish')
    """
    直接使用连表操作将两张表连接起来，并且全部封装成为对象返回。
    无论是要重新查找book表里面的内容还是要查找publish表里面的内容，都不需要在重新走数据库。
    但是使用models.Book.objects.all()来查询的话，不是使用连表查询。如果想要查询publish表里面的内容，
    就需要重新再走一边数据库。
    select_related括号里面只能放外键字段。只能放一对一、一对多
    """

    rev = models.Book.objects.prefetch_related('publish')
    """
    prefetch_related内部是使用子查询，只不过ORM将子查询的结果也给你封装到了对象中。
    给你的感觉好像也是一次查询出来。
    """

"""
上面两种方法，不能说谁的查询速度更加快，要看情况，如果数据量特别大，拼表时间就会比较长，那么子查询的速度就会比较快。
"""
```

### choices参数（数据库常用字段设计）

```python
"""
用户表
	性别、学历、工作经验等可以完全列举可能性字段，我们如何存储。
"""
class User(models.model):
    usename = CharField(max_length=32, verbose_name="用户名")
    gender_choices = (
    (1,"male"),(2,"famale"),(3,"other")
    )
    gender = models.IntegerField(choices=gender_choices)
    
"""
该字段存储的是数字，如果存储的数字在元组的列举范围之内，那么就可以非常轻松的获得数字对应的元组内容。

1.如果gender存储的数字不在元组列举范围以内那么会如何？
存储的时候没有在元组列举的数据里面也可以储存

2. 如何获取到对应的中文信息？
只要是choices参数的字段，如果想要获取对应的信息，那么固定写法是
get_字段名_display()

user_obj = models.User.objects.filter(pk=1).first()
print(user_obj.get_gender_display())

3.如果查找没有对应关系的存储信息，那么查找出来的是什么就返回什么。
"""
# choices使用场景是非常多的
```

### MTV模型和MVC模型

```python
# MTV模型：django号称是MTV模型
M：models
T：templates
V：views

# MVC模型：django本质上还是MVC模型
M：models
V：views
C：controller	
```

### 多对多三种创建方式

```python
# 全自动：利用ORM自动帮我们创建第三张关系表
class Book(models.Model):
    title = models.CharField(max_length=32, verbose_name='书籍名称')
    price = models.DecimalField(max_digits=8, decimal_places=2, verbose_name='书籍价格')
    # 多对多外键字段
    author = models.ManyToManyField(to='Author')
    
class Author(models.Model):
    author_name = models.CharField(max_length=32, verbose_name='作者名字')
    age = models.IntegerField()
    
"""
全自动的优点和缺点：
	优点：代码不需要你写，方便还支持ORM提供的第三张表的操作方法
	缺点：拓展性差，不支持添加额外的字段	
"""
# 纯手动：
class Book(models.Model):
    title = models.CharField(max_length=32, verbose_name='书籍名称')
    price = models.DecimalField(max_digits=8, decimal_places=2, verbose_name='书籍价格')
class Author(models.Model):
    author_name = models.CharField(max_length=32, verbose_name='作者名字')
    age = models.IntegerField()
    
class BookAuthor(models.Model):
    id = models.AutoField(primary_key=True)
    book_id = models.ForeignKey(to='Book', on_delete=models.CASCADE)
    author_id = models.ForeignKey(to='Author', on_delete=models.CASCADE)
    
"""
纯手动创建的可拓展性高。但是需要写的代码比较多。还有就是不能使用ORM提供的简单方法了（add、set、remove、clear）。不能使用正反向查询。不建议使用纯手动的方式创建。
"""
    
# 半自动
class Book(models.Model):
    title = models.CharField(max_length=32, verbose_name='书籍名称')
    price = models.DecimalField(max_digits=8, decimal_places=2, verbose_name='书籍价格')
    # 告诉ORMBook和Author是多对多的关系，并且让ORM不要创建表只需要让Book Author表可以使用ORM中的简单方法来查询就好。
    author = models.ManyToMany(to='Author', through='BookAuthor', through_fields=('book','author'))  # 元组里面字段顺序，外键字段放在那张表，那么关联那张表的字段就放在前面。
class Author(models.Model):
    author_name = models.CharField(max_length=32, verbose_name='作者名字')
    age = models.IntegerField()
    
class BookAuthor(models.Model):
    id = models.AutoField(primary_key=True)
    book = models.ForeignKey(to='Book', on_delete=models.CASCADE)
    author = models.ForeignKey(to='Author', on_delete=models.CASCADE)
"""
半自动的缺点是不能使用ORM提供的add、set、clear、remove等简单的方法。但是可以使用正反向查询。
半自动的拓展性比较高，一般来说我们创建表会使用半自动的方式来创建。
"""
# 总结：上面的方法只需要会全自动和半自动就行了。纯手动不建议使用。
```

## Ajax（使用频率很高）

```python
"""
异步提交
局部刷新

案例：
	GitHub注册案例：动态获取用户输入的数据，拿去后端校验，并且返回给前端展示。
"""
特点：Ajax最大的特点就是在不刷新页面的情况下，可以实现与服务器交换数据并且更新部分网页内容。（在用户不知不觉的情况下就完成了响应和请求。）

我们只学习jQuery封装的，不学习原生的Ajax，所以我们在写前端页面的时候要确保导入了jQuery。
不是只有jQuery实现了Ajax，其他的框架也有，但是换汤不换药。原理是一样的。只是feng'zhuan
```

### Ajax基本语法

```python
# 使用Ajax进行交互的话，无论返回什么，都不会直接作用于浏览器，而会返回给回调
# 如果返回的是字典或列表等数据类型，应该将其进行序列化在传到前端页面上去。返回到前端的也是字符串形式，想要使用方法对他进行操作就要先进行反序列化操作。但是如果你配置了参数datatype: true的话，那么就会自动的帮你把传输过来的数据进行反序列化。
```

#### Ajax小案例（发送json格式shu'ju）

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Ajax request</title>
    <link rel="stylesheet" href="/static/bootstrap-3.4.1-dist/css/bootstrap.min.css">
</head>
<body>
<input type="text" id="1">+
<input type="text" id="2">=
<input type="text" id="3">
<button id="4">提交</button>
<script src="/static/js/jQuery.js"></script>
<script>
    $('#4').click(function(){
        //超后端发送Ajax请求
        data1 = $('#1').val()
        data2 = $('#2').val()
        $.ajax({
            //指定朝那个路径发送请求
            url:'', //如果不写就是默认朝当前地址提交请求
            type: 'post', //指定提交数据的方式
            data: {'username': data1, 'password': data2}, //指定提交的数据
            dataType: 'json', //会自动帮你反序列化后端传回来的数据
            success: function(args){
                $('#3').val(args);
                alert(args)
            },// 回调函数，当后端返回结果的时候自动触发，args就是接受后端返回结果的变量
            })
    })
</script>
</body>
</html>
```

### 前后端传输数据的编码格式

```python
"""
前后端发送请求的编码格式
	urlencoded
	
	formdata
	
	json

"""
# form表单默认的编码格式是urlencoded
"""
数据格式：字符串
	django后端针对符合urlencoded编码格式的数据都会自动帮你解析封装到request.POST中。

	如果你把编码格式改成formdata针对普通的键值对还是解析到POST中，但是会将文件解析到request.FILES中。
	
	form表单是没有办法发送json格式数据的
	
"""
# Ajax
"""
Ajax发送的请求默认也是urlencoded
"""
```

### Ajax发送json格式数据

```python
"""
前后端传输数据的时候一定要确保编码格式跟真正的数据格式是一致的。
"""
contentType:'application/json',加上这个参数，那么传输的数据格式就是json数据格式。
注意：在发送数据之前需要将前端的数据转化成为json格式数据
data: JSON.stringify({'username': 'jason', 'password': '123'})

request.is_ajax()
	判断当前请求是否是ajax请求，返回布尔值。
django针对json格式的数据不会做任何处理。在request.body里面。
request.body拿到的是二进制格式的json格式字符串。
```

### Ajax发送文件

```js
$('#3').click(function () {
        //利用formData内置对象
        let formDataObj = new FormData();
        // 添加普通键值对
        formDataObj.append('username',$('#1').val());
        formDataObj.append('password', $('#2').val());
        //添加文件对象
        formDataObj.append('files', $('#4')[0].files[0]);
        //将对象基于Ajax发送给后端
        $.ajax({
            url:'',
            type: 'post',
            data: formDataObj,//直接将formDataObj对象放进去
            contentType:false, //不使用任何编码，django后端能够自动识别formData对象
            processData:false,//告诉浏-览器不要对数据进行任何的处理
            success:function (args) {

            }
        })
    })
// ajax发送的formData对象封装在request.POST里面，文件对象封装在request.FILES里面。
```

### django自带的序列化组件

```python
# 需求：在前端获得后端用户表里面所有的数据，并且要是列表套字典
"""
以后开发前后端分离项目作为后端的你只需要写代码将数据处理好就行了。
能够序列化返回给前端即可。
	再写一个接口文档告诉前端每一个字段的意思即可。
"""
# 要注意：json不支持时间类型的数据返回date、datetime等。
def send_json(request):
    user_lis = models.User.objects.all()
    print(user_lis)
    """
    序列化。会自动的帮你将数据格式变成json格式的字符串。并且内部十分的全面。
    后端写接口就是利用序列化组件渲染数据，然后写一个接口文档。
    """
    json_obj = serializers.serialize('json', user_lis, ensure_ascii=False)
    print(json_obj)
    return HttpResponse(json_obj)
```

### 批量插入数据

```python
"""
当频繁的走数据库的时候，效率会呈现指数性的下降
"""

# user_lis = []
    # for i in range(10000):
    #     user_obj = models.User.objects.create(name='第%s个用户' % i)
    #     user_lis.append(user_obj)
    # print(user_lis)
    # models.User.objects.bulk_create(user_lis)
    # user_obj = models.User.objects.all()
    # print(user_obj)
```

### 自定义分页推导过程

```python
"""
一般来说页码的设计都是基数，因为符合中国人的对称美

django自定义的分页器书写起来不叫麻烦，并且功能简单、
所以我们需要自定义分页器

"""
```

#### 自定义分页器

```python
class Pagination(object):
    def __init__(self, current_page, all_count, per_page_num=2, pager_count=11):
        """
        封装分页相关数据
        :param current_page: 当前页
        :param all_count:    数据库中的数据总条数
        :param per_page_num: 每页显示的数据条数
        :param pager_count:  最多显示的页码个数
        """
        try:
            current_page = int(current_page)
        except Exception as e:
            current_page = 1
 
        if current_page < 1:
            current_page = 1
 
        self.current_page = current_page
 
        self.all_count = all_count
        self.per_page_num = per_page_num
 
        # 总页码
        all_pager, tmp = divmod(all_count, per_page_num)
        if tmp:
            all_pager += 1
        self.all_pager = all_pager
 
        self.pager_count = pager_count
        self.pager_count_half = int((pager_count - 1) / 2)
 
    @property
    def start(self):
        return (self.current_page - 1) * self.per_page_num
 
    @property
    def end(self):
        return self.current_page * self.per_page_num
 
    def page_html(self):
        # 如果总页码 < 11个：
        if self.all_pager <= self.pager_count:
            pager_start = 1
            pager_end = self.all_pager + 1
        # 总页码  > 11
        else:
            # 当前页如果<=页面上最多显示11/2个页码
            if self.current_page <= self.pager_count_half:
                pager_start = 1
                pager_end = self.pager_count + 1
 
            # 当前页大于5
            else:
                # 页码翻到最后
                if (self.current_page + self.pager_count_half) > self.all_pager:
                    pager_end = self.all_pager + 1
                    pager_start = self.all_pager - self.pager_count + 1
                else:
                    pager_start = self.current_page - self.pager_count_half
                    pager_end = self.current_page + self.pager_count_half + 1
 
        page_html_list = []
        # 添加前面的nav和ul标签
        page_html_list.append('''
                    <nav aria-label='Page navigation>'
                    <ul class='pagination'>
                ''')
        first_page = '<li><a href="?page=%s">首页</a></li>' % (1)
        page_html_list.append(first_page)
 
        if self.current_page <= 1:
            prev_page = '<li class="disabled"><a href="#">上一页</a></li>'
        else:
            prev_page = '<li><a href="?page=%s">上一页</a></li>' % (self.current_page - 1,)
 
        page_html_list.append(prev_page)
 
        for i in range(pager_start, pager_end):
            if i == self.current_page:
                temp = '<li class="active"><a href="?page=%s">%s</a></li>' % (i, i,)
            else:
                temp = '<li><a href="?page=%s">%s</a></li>' % (i, i,)
            page_html_list.append(temp)
 
        if self.current_page >= self.all_pager:
            next_page = '<li class="disabled"><a href="#">下一页</a></li>'
        else:
            next_page = '<li><a href="?page=%s">下一页</a></li>' % (self.current_page + 1,)
        page_html_list.append(next_page)
 
        last_page = '<li><a href="?page=%s">尾页</a></li>' % (self.all_pager,)
        page_html_list.append(last_page)
        # 尾部添加标签
        page_html_list.append('''
                                           </nav>
                                           </ul>
                                       ''')
        return ''.join(page_html_list)
```

+ 后端

```python
 def get_book(request):
   book_list = models.Book.objects.all()
   current_page = request.GET.get("page",1)
   all_count = book_list.count()
   page_obj = Pagination(current_page=current_page,all_count=all_count,per_page_num=10)
   page_queryset = book_list[page_obj.start:page_obj.end]
   return render(request,'booklist.html',locals())
```

+ 前端

```html
<div class="container">
    <div class="row">
        <div class="col-md-8 col-md-offset-2">
            {% for book in page_queryset %}
            <p>{{ book.title }}</p>
            {% endfor %}
            {{ page_obj.page_html|safe }}
        </div>
    </div>
</div>
```

#### 自定义分页器使用

```python
"""
当我们用到非django内部的第三方组件或者代码时，我们一般会创建一个名为utils的文件夹，在该文件夹内对模块进行功能性划分。
我们在后期封装代码的时候不在局限于函数封装，而是尽量的使用类来封装
"""
# 分页器的使用
# 后端使用
book_queryset = models.Book.objects.all()
current_page = request.GET.get('page', 1)  # 如果没有查询到page参数那么就会给值为1.
all_count_data = book_queryset.count()
# 实例化分页器对象
page_obj = Pagenation(current_page=current_page, all_count=all_count_data)
page_queryset = book_queryset[page_obj.start:page_obj.end]
# 只需要将page_obj和page_queryset传递给html页面就行了。

# 前端使用(自定义分页器的使用需要使用bootstrap在使用之前需要保证bootstrap导入了)
{{page_obj.page_html|safe}}

```

### ajax结合sweetalert实现二次确认

```python

```

## form组件

```python
"""
form组件能够完成的事情
	1.渲染html代码
    2.校验数据
    3.展示提示信息
"""
```

### 前戏

```python
"""
写一个注册功能
	获取用户名和密码 利用form表单提交数据
	在后端对数据进行判断
		用户名中不能含有金瓶梅
		密码不能少于三位
    如果不符合条件需要你将	提示信息展示到前端页面
    
"""    
"""
为什么数据校验非要去后端，前端不可以完成吗？
	前端的数据校验可有可无
	但是后端的数据校验必须有！！！ 

因为前端的校验是弱不禁风的。
你可以直接修改或者使用爬虫程序绕过前端页面直接朝后端提交数据。

"""
```

### 基本使用

```python
from django import forms
# 书写form类
class MyForm(forms.Form):
    """
    username长度最小三位最大八位password也一样
    """
    username = forms.CharField(min_length=3, max_length=8)
    password = forms.CharField(min_length=3, max_length=8)
    # email字段必须符合邮箱格式
    email = forms.EmailField()

```

### forms组件校验

```python
"""
1.测试环境的准备可以自己拷贝代码
2.pycharm里面已经帮你准备了一个测试环境
	在pycharm里面的下面python Console里面，你就可以测试django里面任何文件
"""
from app01 import views 
form_obj = views.MyForm({'username': 'jason', 'password': '123', 'email': '123'})
form_obj.is_valid()  # 验证全部数据的合法性，只有全部数据都合法才会返回true。
False
form_obj.cleaned_data  # 拿到全部合法的数据
{'username': 'jason', 'password': '123'}
form_obj.errors # 拿到不合法的数据字段和不合法的原因
{'email': ['Enter a valid email address.']}

# form组件校验规则
"""
校验数据指挥校验类里面出现的字段，不会校验多传的字段。多传的字段会自动忽略。校验数据在默认情况下是每个字段都必须传值的。如果不传值也会返回false。
"""

# 针对字段校验方式有很多种，
	1.最简单的min_length和max_length
    2.正则validator
    3.钩子函数
```

### forms组件html渲染

```python
"""
form组件只会帮你渲染获取用户输入的标签。（input、select、checkbox、radio）不会帮你渲染提交按钮。
"""

# 第一种方式
    # 前端
    <form action="" method="post">
        {{ form_obj.as_p }}
        {{ form_obj.as_ul }}
        {{ form_obj.as_table }}
    </form>
    
"""
第一中方式代码书写少，但是封装程度比较高，不利于后续拓展。一般情况下只是本地测试的时候使用。
"""

# 第二种拓展方式
    {{ form_obj.username.label }}:{{ form_obj.username }}
    {{ form_obj.password.label }}:{{ form_obj.password }}
    {{ form_obj.email.label }}:{{ form_obj.email }}
"""
第二种方式的可拓展性很强，但是需要书写的代码太多一般情况下不用
"""

# 第三种方式（推荐使用）
	{% for form in form_obj %}
        <p>{{ form.label }}:{{ form }}</p>
    {% endfor %}
"""
第三种方式代码书写简单，扩展性强推荐使用。
"""

# 三种方式的后端都是：
    def index(request):
        # 先产生一个空对象
        form_obj = MyForm()
        return render(request, 'index.html', locals())
```

### 展示提示信息

```python
"""
浏览器会自动的帮你校验数据
	如何让浏览器不帮你校验数据？
	只需要在form表单中加上novalidate就行了
"""

# 自定义报错信息
class MyForm(forms.Form):
    """
    username长度最小三位最大八位password也一样
    """
    username = forms.CharField(min_length=3, max_length=8, label='用户名', error_messages={
        'min_length': '长度小于三',
        'max_length': '长度大于八',
        'required': '用户名不能为空',
    })
    password = forms.CharField(min_length=3, max_length=8, error_messages={
        'min_length': '长度小于三',
        'max_length': '长度大于八',
        'required': '密码不能为空',
    })
    # email字段必须符合邮箱格式
    email = forms.EmailField(error_messages={
        'invalid': '邮箱格式不正确',
        'required': '邮箱不能为空',
    })

# 展示报错信息后端写法
def index(request): 
    # 先产生一个空对象
    form_obj = MyForm()
    if request.method == 'POST':
        form_obj = MyForm(request.POST)  # 将字典传给MyForm类里面，产生MyForm对象
        if form_obj.is_valid():
            return HttpResponse("OK")
        return render(request, 'index.html', locals())
    return render(request, 'index.html', locals())

# 前端写法
 {% for form in form_obj %}
        <p>
            {{ form.label }}:{{ form }}
            <span style="color:red">{{ form.errors.0 }}</span>
        </p>

    {% endfor %}
    <input type="submit" class="btn btn-info">
```

### 钩子函数（hook）

```python
"""
在特定的节点自动触发，完成响应操作
钩子函数类似于form组件中的第二道关卡，能够让我们自定义校验规则。
form组件有两种钩子
	1.局部钩子
		当你需要给单个字段增加校验规则的时候可以使用
	2.全局钩子
		当你需要给多个字段增加校验规则的时候可以使用
"""
# 实际案例：
# 用户名中不能含有666			只是校验username字段	使用局部钩子

# 校验密码和确认密码是否一致		 需要确认password和confirm_password需要确认两个字段	使用全局钩子

"""
钩子函数需要第一道前面的校验通过以后才会触发。
"""
 # 局部钩子
    def clean_username(self):
        # 将用户名拿去出来
        username = self.cleaned_data.get('username')
        # 对用户名进行再次校验
        if "666" in username:
            self.add_error('username', '光喊666是不行的，还需要自己牛逼~')
        # 将钩子函数调出来的数据再放回去
        return username

    # 全局钩子
    def clean(self):
        password = self.cleaned_data.get('password')
        confirm_password = self.cleaned_data.get('confirm_password')
        if password != confirm_password:
            self.add_error('confirm_password', '两次密码不一致，请再次确认！')
        # 将所有的被钩子函数拿出来的数据放回去。
        return self.cleaned_data
```

### forms组件补充知识点

```python
label  字段名
error_message = {} 自定义报错信息
initial  默认值
required = False  控制字段是否必填

"""
1.字段没有样式
2.针对不同的input如何修改	
"""
# 可以通过widget参数来修改	
username = forms.CharField(min_length=3, max_length=8, label='用户名', error_messages={
        'min_length': '长度小于三',
        'max_length': '长度大于八',
        'required': '用户名不能为空'},
        widget=forms.widgets.TextInput(attrs={'class': 'form-control'}),  # 将标签的type变成text
                          )
# 在括号里面你可以通过attrs参数操作任何属性，铜鼓传键值对的方式。如果class后面需要多个参数，那么直接加空格在后面写就行了。

# 正则校验
from django.core.validators import RegexValidator
phone = forms.CharField(validators=[
        RegexValidator('^[0-9]{11,11}$', '请输入数字'),
        RegexValidator(r'^173[0-9]{8,8}$', '电话必须要以173开头')
    ],
        widget=forms.widgets.TextInput(attrs={'class': 'form-control'}))

```

+ 其他字段

```python
# radio select
gender = forms.ChoiceField(
        choices=((1, "男"), (2, "女"), (3, "保密")),
        label="性别",
        initial=3,
        widget=forms.widgets.RadioSelect()
    )

# 单选select
hobby = forms.ChoiceField(
        choices=((1, "篮球"), (2, "足球"), (3, "双色球"), ),
        label="爱好",
        initial=3,
        widget=forms.widgets.Select()

# 多选的select
hobby = forms.MultipleChoiceField(
        choices=((1, "篮球"), (2, "足球"), (3, "双色球"), ),
        label="爱好",
        initial=[1, 3],
        widget=forms.widgets.SelectMultiple()
    )

# 单选的checkbox
keep = forms.ChoiceField(
        label="是否记住密码",
        initial="checked",
        widget=forms.widgets.CheckboxInput()
    )

# 多选的checkbox
hobby = forms.MultipleChoiceField(
        choices=((1, "篮球"), (2, "足球"), (3, "双色球"),),
        label="爱好",
        initial=[1, 3],
        widget=forms.widgets.CheckboxSelectMultiple()
    )
```

## cookie和session

```python
"""
早期的登录功能：不保存用户的登录状态，也就意味着用户每次访问网站都需要登录。需要重复的输入账号和密码。
（这样非常的不方便）
第一个解决方法：
	用户第一次登录成功以后服务端会将账号和密码全部返回给用户的浏览器，让用户的浏览器保存在本地。之后每次访问网站自动将保存在浏览器上面的账号和密码发送给服务端。服务端获取之后自动验证。
	这种方式存在十分大的安全隐患。
	优化版本：
		当用户登录以后服务端随机产生一个字符串交由客户端浏览器保存。之后在访问浏览器的时候都会自动的带着该字符串。
		之后访问服务端的时候都会带着随机字符串，服务端去数据库中比对是否有随机字符串。从而获取	到对应的用户信息。
		但是如果随机字符串被截取到了，那么你就可以冒充当前用户。其实这样还是有安全隐患。
		web领域没有绝对的安全。
"""

"""
cookie
    服务端保存在浏览器上面的信息都可以叫cookie。
    他的表现形式一般都是k:v键值对（可以有多个）。

session
	数据是保存在服务端的并且他的表现形式也是看k:v键值对（可以有多个）。
	
token
	session虽然是保存在服务端的，但是数据量大。
	token就是服务端不在保存数据，而是登录成功之后将一段信息加密处理。

总结：
	cookie就是保存在浏览器上面的信息
	session就是保存在服务端的信息
	session是基于cookie工作的（大部分保存用户状态的操作都是使用cookie）
"""
```

### cookiec操作

```python
"""
虽然cookie是服务端告诉客户端需要保存的内容，但是客户端可以选择拒绝保存。如果拒绝使用cookie那么需要保存cookie的网站就会登录不了。
"""
# 如果想要操作cookie就需要获取视图函数的返回对象，使用返回对象里面的方法来操作cookie。
例如：obj = HttpResponse()
	# 操作cookie
	return obj

"""
设置cookie
	obj.set_cookie(key, value)
获取cookie
	request.COOKIES.get(key)

设置cookie的时候是可以设置超时时间的，在超过这个时间以后cookie就是失效。
obj.set_cookie(key, value, max_age=3)
表示cookie时间只会存在3秒，三秒以后cookie就会失效。
如果设置max_age不起效果，那么就是用expires=3，你这个参数也是设置超时时间的，不过是专门针对IE浏览器的。

删除cookie（类似于注销功能）
	obj.delete_cookie(key)  # 删除cookie
"""

```

### 使用cookie制作一个真正的登录功能

```python
url(r'login/', views.login),
url(r'^home/', views.home)
"""
要求：
    用户想访问那个页面，在用户登录完成以后就给用户跳转到那个页面去
"""
# 装饰器
def cookie_auth(func):
    def inner(request, *args, **kwargs):
        if request.COOKIES.get('jason'):
            return func(*args, **kwargs)
        next_url = request.get_full_path()
        return redirect(f'/login/?next={next_url}')
    return inner


def login(request):
    next_url = request.GET.get('next')
    if next_url:
        if request.method == "POST":
            if request.POST.get('username') == 'jason' and request.POST.get('password') == '123':
                http_obj = redirect(next_url)
                http_obj.set_cookie('jason', 'jason666')
                return http_obj
    return render(request, 'login.html', locals())


def home(request):
    if request.COOKIES.get('jason'):
        return HttpResponse('登录成功')
    return redirect('/login/')

```

### session操作

```python
"""
设置session
	request.session['key'] = value
	
设置session内部发生的事情：
	1.django内部会自动帮你生成一个随机字符串
	2.django会自动将随机字符串存储到django_session表中。
		2.1现在内存中产生操作数据的缓存
		2.2在响应经过django中间件的时候才真正操作数据库。
	3.将产生的随机字符串返回给浏览器保存。	

在默认情况下操作session的时候需要django默认的一张session表了。
你在使用数据库迁移命令的时候django会帮你创建很多表django_session就是其中一张。

django默认session的过期时间是14天。但是你可以人为的修改它。		
"""

"""
获取session
	request.session.get('key')
	
获取session的时候发生的事情：
	1.自动的从浏览器中获取session对应的随机字符串
	2.拿着随机字符串去django_sesion表中查找响应的数据
	3.如果数据比对上，则将数据取出并且封装到request.session中，如果没比对上则request.session.get返回None。
	
session表中数据条数取决于浏览器的，同一个计算机上面的同一个浏览器只会有一条数据生效（当session过期的时候可能会出现多条数据对应一个浏览器，但是该现象不会出现很久，内部会自动清除。也可以通过代码清除）。这样做主要是为了节省数据库的资源。

"""

"""
设置session过期时间
	request.session.set_expiry()
	括号内可以直接放4中参数：
		1.整数		多少秒
		2.日期对象	   指定到多少日期就失效
		3.数字零		一旦当前浏览器窗口关闭就立刻失效
		4.不写		 不写的话就是14天。
"""

"""
清除session
	request.session.delete()  # 只删除服务端的客户端的session不删除。
	request.session.flush()  # 浏览器和服务端都清空（推荐使用）
"""

"""
session是保存在服务端的，但是存储的位置可以有很多选择
	1.mysql
	2.文件
	3.redis
	4.memcache
	等等
"""
# session也可以进行加盐处理
```

### CBV如何添加装饰器

```python
# CBV如何添加装饰器
"""
CBV添加装饰器需要导入method_decorator模块
"""
from django.utils.decorators import method_decorators
# 方式1
class MyLogin(View):
    @method_decorator(cookie_auth)
    def get(self):
        return HttpResponse('get请求！')

    @method_decorator(cookie_auth)
    def post(self):
        return HttpResponse('post请求！')
   
# 方式2
@method_decorators(login_auth, name='get')
@method_decorators(login_auth, name='post')
class MyLogin(View):    
    def get(self):
        return HttpResponse('get请求！')

    def post(self):
        return HttpResponse('post请求！')
    
# 方式3(方式三会直接将类里面所有的方法全部都加上该装饰器)
class MyLogin(View):   
    @method_decorator(cookie_auth) 
    def dispatch(self, request, *args, **kwargs)
    	pass
    def get(self):
        return HttpResponse('get请求！')

    def post(self):
        return HttpResponse('post请求！')
"""
在django中，CBV不建议你直接给类方法加装饰器，无论该装饰器是否正常运行
"""
```

## django中间件

```python
"""
django自带7个中间件，每个中间件都 有各自的功能，并且django还支持程序员自定义中间件。并且暴露给程序员五个可以自定义的方法。
	1.必须掌握
	process_request（使用频率最高的方法，请求来的时候需要走的方法）
		1.请求来的时候是按照settings文件里面从上往下的顺序依次执行的。如果中间件里面没有定义该方法，那么就会直接跳过执行下一个中间件。
		2.如果该方法返回了HTTP Response对象，那么请求就不会在往后执行了，会园路返回。
		3.process_request方法就是用来做全局相关的所有限制功能。
		4.基本上的防止数据爬取的措施都是在process_request里面写的。
		
	process_response
		1.响应走的时候需要经过中间件里面的每一个response方法。该方法有两个额外参数。request和response（就是要返回给浏览器的内容）。该方法必须要返回一个HTTP Response对象。
		2.默认返回的就是response，你也可以自定义内容
		3.响应走的时候是按照配置文件中注册中间件从下往上依次经过。
		4.如果没有定义的话直接跳过执行下一个。
		
    如果在第一个process_request方法就已经返回HTTP Response对象，那么响应走的时候是经过中间件里面所有的process_response方法还是其他？
    	是其他情况，会直接走同级别的process_response返回。
    	flask框架就和django中间件的规律不一样。
    		flask框架只要是返回了数据，就必须经过中间件里面所有列斯与process_response的方法。
	2.了解即可
	process_view
		process_view在路由匹配之后，在视图函数执行之前执行。
	
	process_template_response（基本上不用）
		只有在返回的属性有render的时候才会被触发，顺序是按照配置文件注册了的中间件从上往下执行的。
	
	process_exception
		process_exception是在报错的时候触发的，

在使用django开发项目的时候，只要是涉及到全局的功能都可以使用中间件来完成。
例如：
	全局用户身份校验
	全局用户权限校验
	全局的访问频率校验
"""
"""
django中间件就是django的门户
1.请求要先经过django中间件才能真正的到达后端（请求要一次经过7个中间件）
2.响应走的时候也需要经过中间件才能发出去。
"""
# 七个中间件
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    # 'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]
```

###  自定义中间件

```python
"""
1.在你的项目名或者应用名下创建一个任意名字的文件夹
2.在该文件夹内创建一个任意名字的py文件
3.在该py文件内需要书写一个类（这个类必须要继承MiddlewareMixin）,然后在自定义类中自定义方法。
4.需要将类的名字以字符串的形式注册到配置文件中才能生效。

MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    # 'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
    '自定义中间件路径',
"""

from django.utils.deprecation import MiddlewareMixin


class MyMiddleware(MiddlewareMixin):
    def process_request(self, request):
        pass

    def process_response(self, request, response):
        """
        :param request:
        :param response:response就是django后端返回给浏览器的内容
        :return:
        """
        pass

    def process_view(self, request, view_name, *args, **kwargs):
        pass

    def process_template_response(self, request, response):
        pass

    def process_exception(self, request, exception):
        """
        :param request:
        :param exception: 就是报错信息
        :return:
        """
        pass

```

## csrf跨站请求伪造

```python
# csrf跨站请求伪造校验
	"""
	网站在给用户返回一个具有提交数据功能页面的时候，会给页面添加一个唯一标识，当这个页面朝后端发送post请求的时候我们的后端会先校验唯一标识。如果唯一标识不对，直接拒绝（403 forbidden），否则则正常执行。
	"""
```

```python
# form表单通过csrf校验
"""
	想要通过csrf校验只需要在form表单中任何一个位置加上{% csrf_token %},
这个在页面中就是一个隐藏的input框，name=csrfmiddlewaretoken，value就是一个随机字符串。
"""

# ajax通过csrf校验
"""
方法一：
	利用标签查找获取页面上的随机字符串。键的名字必须要叫csrfmiddlewaretoken
	data : 	{'csrfmoddlewaretoken':$('[name=csrfmiddlewaretakon]').val()}

第二种方式：
	直接利用模板语法提供的快捷方式
	data : 	{'csrfmoddlewaretoken':'{{csrf_token}}'}
	
第三种方式：（通用方式）
	function getCookie(name) {
    var cookieValue = null;
    if (document.cookie && document.cookie !== '') {
        var cookies = document.cookie.split(';');
        for (var i = 0; i < cookies.length; i++) {
            var cookie = jQuery.trim(cookies[i]);
            // Does this cookie string begin with the name we want?
            if (cookie.substring(0, name.length + 1) === (name + '=')) {
                cookieValue = decodeURIComponent(cookie.substring(name.length + 1));
                break;
            }
        }
    }
    return cookieValue;
}
var csrftoken = getCookie('csrftoken');

function csrfSafeMethod(method) {
  // these HTTP methods do not require CSRF protection
  return (/^(GET|HEAD|OPTIONS|TRACE)$/.test(method));
}

$.ajaxSetup({
  beforeSend: function (xhr, settings) {
    if (!csrfSafeMethod(settings.type) && !this.crossDomain) {
      xhr.setRequestHeader("X-CSRFToken", csrftoken);
    }
  }
});

将以上代码copy到静态文件里面去，在将文件引入到发送的html文件里面去。就行了。
"""
```

### csrf相关装饰器

```python
"""
整个网站都需要校验有几个不需要校验
整个网站都不需要校验，但是有一些需要校验
"""
from django.view.decorators.csrf import csrf_protect, csrf_exempt
	# 针对csrf_exempt装饰器，只有给dispatch添加装饰器才能生效。
    # csrf_process可以使用上面学的三种方法里面的任何一种。
```

### 补充知识点

```python
# importlib模块
import importlib
res = "myfile.b"
# 该方法最小只能到py文件
importlib.import_module(res)  # 相当于from myfiel import b
```

### 基于中间件学习的编程思想

```python
# settings文件里面的内容
MYFILES_DIR = [
    'myfiles.qq.QQ',
    'myfiles.wechat.Wechat'

]
# start文件里面的内容
import myfiles
myfiles.send_all('快下课了！')

# myfiles文件夹init里面的内容
import importlib
import settings


def send_all(content):
    for path_str in settings.MYFILES_DIR:
        # 拿到settings文件里面的路径
        file_path, cls_name = path_str.rsplit('.', maxsplit=1)
        file_obj = importlib.import_module(file_path)
        # 利用反射获取类名
        cls = getattr(file_obj, cls_name)
        # 利用鸭子类型直接调用send方法
        obj = cls(content)
        obj.send()
 
# myfiles文件夹内qq.py里面的内容
class QQ:
    def __init__(self, content):
        self.content = content

    def send(self):
        print(f'qq消息:{self.content}')

"""


"""
```

## Auth模块

+ 只要是和用户登录、注册、校验、修改密码、注销、验证用户是否登录的功能Auth模块都能帮你做。

```python
# 导入Auth模块
from django.contrib import auth

# 使用Auth模块创建超级用户（管理员）
	python manage.py createsuperuser

# 查找auth_user表，给密码加密在比对（必须要将用户名和密码全部传进去）
res = auth.authenticate(request, username=username, password=password)  

# 这个方法返回的是用户对象,可以使用用户对象来点属性，获取用户的用户名和密码。（如果输入的用户名和密码不正确，那么就会返回none）
print(res)  # 打印的是用户名
res.username
res.password

# 操作django_session表，保存用户数据（只要执行了该方法，你就可以在任何地方使用request.user 获取到当前登录的用户对象）
# 相当于 request.session['key'] = res
auth.login(request, res)  

# 如果用户登录就会返回一个用户对象，如果用户没有登录，那么就会就会返回AnonymousUser。
request.user

# 判断用户是否登录
request.user.is_authenticated()

# django自带登录验证装饰器
from django.contrib.auth.decarators import login_required

# 局部配置
@login_required(login_url='/login/')  # login_url可以指定跳转的页面。
def home(request):
    return render(request, 'home.html')

# 全局配置
在settings文件里面配置
LOGIN_URL = '/login/'
@login_required
def home(request):
    return render(request, 'home.html')

# 如果局部和全局全都配置了跳转页面，那么局部的跳转优先级更加高。

# 校验密码（修改密码时校验）
is_right = request.user.check_password(old_password) # 校验输入的密码是否正确。

# 修改密码
request.user.set_password(new_password)
request.user.save()  #将修改结果提交到数据库

# 注销
def loguot(request):
    auth.logout(request)  # 类似于request.session.flush()
    return redirect('/login/')

# 注册用户
from django.contrib.auth.models import User

def register(request):
    if request.method == 'POST':
        username = request.POST.get('username')
		password = request.POST.get('password')
        User.objects.create(username=username, password=password)  # 写入数据（这样写入数据密码是明文的）
        User.objects.create_user(username=username, password=password) 
        # 创建超级用户(邮箱是必填的，创建超级用户，但是一般我们不会把这个创建超级用户的接口暴露给用户。)
        User.objects.create_superuser(username=username, email='123@qq.com', password=password,)
        
```

### Auth模块表拓展

```python
# 可以使用一对一方式拓展
# 第一种方式（不推荐）
import django.contrib.auth import models
import django.db import models

class UserDetail(models.Model)
	phone = models.CharField(max_length=11)
    user = models.OnetoOneField(to='User')
    
# 第二种方式
# 使用面向对象的继承
from django.contrib.auth.models im AbstractUser
class UserInfo(AbstractUser):
    """
    如果你继承AbstractUser
    那么在执行数据库迁移命令的时候就不会创建auth_user表
    而UserInfo表中就会继承啊auth_user表的中所有数据，并且会加上自己拓展的字段。
    
    前提：
    1.在继承之前没有执行过数据库迁移命令（auth_user表没有被创建出来）
    2.继承的类里面不要覆盖AbstractUser里面的字段名字
    	里面的字段不要动，之扩展额外的字段即可
    3.在配置文件中告诉django你要是用UserInfo代替auth_user表
    	AUTH_USER_MODEL = 'app01.UserInfo'  '应用名.表名'
    """
    phone = models.CharFiled(max_length=11)
    
```



