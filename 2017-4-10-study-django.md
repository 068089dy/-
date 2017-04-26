---
layout: post
title:  "django学习"
category: "note"
keywords: ["python", "django","web"]
description: "django学习"
tags: ["python", "django","web"]
---
<h2> 前言</h2>

自己做的笔记，详细的教程在[这里](http://wiki.jikexueyuan.com/project/django-set-up-blog/)

<h2> 基本命令</h2>


下载1.10.6版本django

{% highlight shell %}
pip3 install django==1.10.6
{% endhighlight %}
卸载django
{% highlight shell %}
pip3 uninstall djang
{% endhighlight %}
查看版本
{% highlight shell %}
python
import django
print django.VERSION
{% endhighlight %}
帮助文档
{% highlight shell %}
python manange.py -h
{% endhighlight %}
## 使用
### 1.创建项目
{% highlight shell %}
django-admin.py startproject my_blog
{% endhighlight %}
### 2.建立Django app
{% highlight shell %}
python manage.py startapp article
{% endhighlight %}
//建立Django app要在my_blog/my_blog/setting.py下添加新建app
{% highlight shell %}
INSTALLED_APPS = (
    ...
    'article',  #这里填写的是app的名称
)
{% endhighlight %}
### 3.创建超级用户
{% highlight shell %}
python manage.py createsuperuser
{% endhighlight %}
创建超级用户之后，可以访问127.0.0.1：8000/admin访问
### 4.关于url
创建app后，我们可以把在app/views里添加方法，Eg.
{% highlight python %}
from django.shortcuts import render
from django.http import HttpResponse
#Create your views here.
def shouye(request):
    return HttpResponse("Hello World, Django,qweqwe")
{% endhighlight %}
然后，再在project/project/urls里添加相应的链接
{% highlight python %}
...
import app.views
urlpatterns = [
    ...
    url(r'^$',app.views.shouye),  #访问url
    url(r'^home/',app.views.shouye),  #访问url/home
    url(r'^([a-z，0-9]+)/$', article.views.detail, name='detail'),#访问url/数字与字母的组合
]
{% endhighlight %}
这样我们在访问url时就会返回 Hello World, Django,qweqwe。

### 4.1.返回html
上面只是返回字符串，如果想要返回一个网页，可以按如下步骤操作：
先写一个网页，在project目录下新建一个template目录，在目录下建一个html文档

再在project/project/setting中修改TEMPLATES
{% highlight python %}
TEMPLATES = [
    {
        ...
        'DIRS': [os.path.join(BASE_DIR, 'templates')],
        ...
    },
]
{% endhighlight %}
老版本是这样：
{% highlight python %}
TEMPLATE_DIRS = (
    ...
    [os.path.join(BASE_DIR, 'templates')],
    ...
)
{% endhighlight %}
然后在app/views里添加方法：
{% highlight python %}
def test(request) :
        return render(request, 'test.html', {'current_time': datetime.now()})
{% endhighlight %}
然后，再在project/project/urls里添加相应的链接
{% highlight python %}
url(r'^test/',app.views.test),
{% endhighlight %}

### 4.2.添加静态文件夹以便添加样式（[原文](www.ziqiangxuetang.com/django/django-static-files.html)）
在project目录下新建一个static目录，在setting中做如下设置：
{% highlight python %}
STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, 'collected_static')

STATICFILES_DIRS = (
    os.path.join(BASE_DIR, "static"),
)
{% endhighlight %}
在static下建立css和js目录，里面分别存放css和js文件，在网页中调用时，这样操作：
{% highlight html %}
<link href="/static/css/bootstrap.css" rel="stylesheet">
<script src="/static/js/jquery.js"></script>
{% endhighlight %}
### 5.post请求处理
这是处理get请求，如果是post请求，就这样：
{% highlight python %}
def login(request) :
    if request.method == 'POST':# 当提交表单时
        u = request.POST['u']
        p = request.POST['p']
        return HttpResponse("user"+u+" "+"password:"+p)
    else:# 当正常访问时
        return render(request, 'login.html', {'current_time': datetime.now()})
{% endhighlight %}
网页中表单的格式：
{% highlight html %}
<form method="post">
      {\% csrf_token \%}
      #{{ form }}
      <input type="text" required="required" placeholder="用户名" name="u"></input>
      <input type="password" required="required" placeholder="密码" name="p"></input>

      <button class="but" type="submit">登录</button>

</form>
{% endhighlight %}
表格后面还有一个{/% csrf_token /%}的标签(%前没有/，不知道怎么转义)。csrf 全称是 Cross Site Request Forgery。这是Django提供的防止伪装提交请求的功能。POST 方法提交的表格，必须有此标签。

### 6.创建models
{% highlight python %}
from django.db import models
    # Create your models here.
    class Article(models.Model) :
        head_image = models.CharField(max_length = 100)  #头像链接
        name = models.CharField(max_length = 50, blank = True)  #名
        date_time = models.DateTimeField(auto_now_add = True)  #日期
        content = models.TextField(blank = True, null = True)  #正文

        def __unicode__(self) :
            return self.name

        class Meta:  #按时间下降排序
            ordering = ['-date_time']
{% endhighlight %}

其中__unicode__(self) 函数Article对象要怎么表示自己, 一般系统默认使用`` 来表示对象, 通过这个函数可以告诉系统使用title字段来表示这个对象
CharField 用于存储字符串, max_length设置最大长度
TextField 用于存储大量文本
DateTimeField 用于存储时间, auto_now_add设置True表示自动设置对象增加时间

### *同步数据库
{% highlight shell %}
python manage.py makemigrations
{% endhighlight %}
下面这俩是一个功能，只是版本不同
{% highlight shell %}
python manage.py migrate
python manage.py syncdb
{% endhighlight %}
### *使用shell操作model

#### *进入Django中的交互式shell来进行数据库的增删改查等操作
{% highlight shell %}
python manage.py shell
{% endhighlight %}
#### *create数据库增加操作
{% highlight shell %}
>>> Article.objects.create(title = 'Hello World', category = 'Python', content = '我们来做一个简单的数据库增加操作')
{% endhighlight %}
#### *all和get的数据库查看操作
{% highlight shell %}
>>> Article.objects.all()  #查看全部对象, 返回一个列表, 无对象返回空list,不显示内容
>>> Article.objects.get(id = 1)  #返回符合条件的对象
{% endhighlight %}
#### *update数据库修改操作
{% highlight shell %}
>>> first = Article.objects.get(id = 1)  #获取id = 1的对象
>>> first.name='....'
{% endhighlight %}
#### *delete数据库删除操作
{% highlight shell %}
>>> first.delete()
{% endhighlight %}

### 7.部署到nginx上

安装uwsgi和nginx

{% highlight shell %}
pip3 install uwsgi
sudo apt-get install nginx
{% endhighlight %}

#### 7.1.uwsgi

测试

新建一个文件test.py,内容如下：

{% highlight python %}
def application(env, start_response):
    start_response('200 OK', [('Content-Type','text/html')])
    return [b"Hello World"]
{% endhighlight %}

然后执行

{% highlight shell %}
uwsgi --http :8001 --wsgi-file test.py
{% endhighlight %}

然后进入浏览器访问127.0.0.1:8001，浏览器返回“Hello World”就没问题了

新建一个uwsgi配置文件，我命名为my_uwsgi.ini，根据实际情况填写如下内容

{% highlight shell %}
[uwsgi]
user nginx;       #用户名，这个貌似可以随意填
socket=0.0.0.0:8005   #这个要与下面的nginx配置文件中的uwsgi_pass相同
chdir=/home/dy/github/project   #django工程目录
wsgi-file=project/wsgi.py     #工程目录下的wsgi.py文件
touch-reload=/home/dy/github/project/reload     
pythonpath = /usr/bin/python3

processes=2         #两个进程
threads=4           #每个进程4个线程

chmod-socket=664
{% endhighlight %}

然后执行my_uwsgi.ini

{% highlight shell %}
uwsgi --ini my_uwsgi.ini
{% endhighlight %}

#### 7.2.nginx

更改/etc/nginx/nginx.conf

{% highlight shell %}

http{

......

  server{
  		listen 127.0.0.1:80;    #监听本机80端口
  		server_name example.com;  #域名
  		charset utf-8;          #编码

  		client_max_body_size 75M;

  		location /static {
  			alias /home/dy/github/myblog1/static/;   #静态文件设置
  		}

  		location /{
  			uwsgi_pass 0.0.0.0:8005;
  			include /etc/nginx/uwsgi_params;         #不知道是啥玩意
  		}
  	}
}
{% endhighlight %}

然后，重启或重载文件

{% highlight shell %}

>>> service nginx restart

>>> service nginx reload

{% endhighlight %}

这样只能在本机访问，如果要让其他人访问，需要注释server中的这一行：

{% highlight shell %}
#listen 127.0.0.1:80;    #监听本机80端口
{% endhighlight %}
