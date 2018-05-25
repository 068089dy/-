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

```
pip3 install django==1.10.6
```
卸载django
```
pip3 uninstall djang
```
查看版本
```
python
import django
print django.VERSION
```
帮助文档
```
python manange.py -h
```
## 使用
### 1.创建项目
```
django-admin.py startproject my_blog
```
### 2.建立Django app
```
python manage.py startapp article
```
//建立Django app要在my_blog/my_blog/setting.py下添加新建app
```
INSTALLED_APPS = (
    ...
    'article',  #这里填写的是app的名称
)
```
### 3.创建超级用户
```
python manage.py createsuperuser
```
创建超级用户之后，可以访问127.0.0.1：8000/admin访问
#### 重设admin user密码
```
from django.contrib.auth.models import User
user =User.objects.get(username='admin')
user.set_password('new_password')
user.save()
```
### 4.关于url和模板（template）
创建app后，我们可以把在app/views里添加方法，Eg.
```
from django.shortcuts import render
from django.http import HttpResponse
#Create your views here.
def shouye(request):
    return HttpResponse("Hello World, Django,qweqwe")
```
然后，再在project/project/urls里添加相应的链接
```
...
import app.views
urlpatterns = [
    ...
    url(r'^$',app.views.shouye),  #访问url
    url(r'^home/',app.views.shouye),  #访问url/home
    url(r'^([a-z，0-9]+)/$', article.views.detail, name='detail'),#访问url/数字与字母的组合
]
```
这样我们在访问url时就会返回 Hello World, Django,qweqwe。
#### 多级url
比如127.0.0.1/blog/test这样的url：
1.先在project/app目录下建立urls.py文件。
2.然后在project/project/usrls中添加语句(__注意导入include__)：
```
from django.conf.urls import url,include
from django.contrib import admin

import article.views

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    ...
    url(r'^blog/',include('app.urls')),
    ...
]
```
3.然后在project/app/urls.py中添加：
```
from django.conf.urls import url,include
from django.contrib import admin
import app.views

urlpatterns = [
    #url(r'^admin/', admin.site.urls),
    url(r'^$', app.views.blog),
    url(r'^([a-z，0-9]+)/', app.views.test),
]
```
这样当我们访问127.0.0.1/blog时，返回的就是app.views.blog的内容，访问127.0.0.1/blog/test (__([a-z，0-9]+)__)时，就返回app.views.test中的内容。
```
#id是“test”，127.0.0.1/blog/“id”
def view(request, id):
    pass
```

#### 多url之间的跳转及数据传递
```
```
#### 高级模板template

### 4.1.返回html（template）
上面只是返回字符串，如果想要返回一个网页，可以按如下步骤操作：
先写一个网页，在project目录下新建一个template目录，在目录下建一个html文档

再在project/project/setting中修改TEMPLATES
```
TEMPLATES = [
    {
        ...
        'DIRS': [os.path.join(BASE_DIR, 'templates')],
        ...
    },
]
```
老版本是这样：
```
TEMPLATE_DIRS = (
    ...
    [os.path.join(BASE_DIR, 'templates')],
    ...
)
```
然后在app/views里添加方法：
```
def test(request) :
        return render(request, 'test.html', {'current_time': datetime.now()})
```
然后，再在project/project/urls里添加相应的链接
```
url(r'^test/',app.views.test),
```

### 4.2.添加静态文件夹以便添加样式（[原文](www.ziqiangxuetang.com/django/django-static-files.html)）
在project目录下新建一个static目录，在setting中做如下设置：
```
STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, 'collected_static')

STATICFILES_DIRS = (
    os.path.join(BASE_DIR, "static"),
)
```
然后执行命令：
```
$ python manage.py collectstatic
```
在static下建立css和js目录，里面分别存放css和js文件，在网页中调用时，这样操作：
```
<link href="/static/css/bootstrap.css" rel="stylesheet">
<script src="/static/js/jquery.js"></script>
```
### 5.post请求处理
这是处理get请求，如果是post请求，就这样：
```
def login(request) :
    if request.method == 'POST':# 当提交表单时
        u = request.POST['u']
        p = request.POST['p']
        return HttpResponse("user"+u+" "+"password:"+p)
    else:# 当正常访问时
        return render(request, 'login.html', {'current_time': datetime.now()})
```
网页中表单的格式：
```
<form method="post">
      {\% csrf_token \%}
      #{{ form }}
      <input type="text" required="required" placeholder="用户名" name="u"></input>
      <input type="password" required="required" placeholder="密码" name="p"></input>

      <button class="but" type="submit">登录</button>

</form>
```
表格后面还有一个{/% csrf_token /%}的标签(%前没有/，不知道怎么转义)。csrf 全称是 Cross Site Request Forgery。这是Django提供的防止伪装提交请求的功能。POST 方法提交的表格，必须有此标签。

### 6.创建models
```
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
```

其中__unicode__(self) 函数Article对象要怎么表示自己, 一般系统默认使用`` 来表示对象, 通过这个函数可以告诉系统使用title字段来表示这个对象
CharField 用于存储字符串, max_length设置最大长度
TextField 用于存储大量文本
DateTimeField 用于存储时间, auto_now_add设置True表示自动设置对象增加时间

#### 外键以及manytomany
ForeignKey
```
from django.db import models
from django.contrib.auth.models import User

# Create your models here.
class head_comment(models.Model):
    title = models.CharField(max_length = 100)
    # 这里的user指向的是django auth框架中的model “User”
    user = models.ForeignKey(User, on_delete=models.CASCADE)
    content = models.TextField()
    tags = models.TextField()
    create_time = models.DateTimeField(auto_now_add = True)

class comment(models.Model):
    # 这里的father_id指向上面的head_comment
    father_id = models.ForeignKey(head_comment, on_delete=models.CASCADE)
    content = models.TextField()
    user = models.ForeignKey(User, on_delete=models.CASCADE)
    '''
    如果一个表中有几个ManyToManyField，则要给每个ManyToManyField指定不同related_name参数。
    而且不同的表中的related_name也不能相同。
    不然会报错：
    HINT: Add or change a related_name argument to the definition for 'comment.thumb_down' or 'comment.thumb_up'.
    mycomment.comment.thumb_down: (fields.E304) Reverse accessor for 'comment.thumb_down' clashes with reverse accessor for 'comment.user'.
    '''
    thumb_up = models.ManyToManyField(User, related_name="comment_thumb_up")
    thumb_down = models.ManyToManyField(User, related_name="comment_thumb_down")
```

#### 将model添加到admin下管理
在project/app/admin.py中添加：
```
from django.contrib import admin
from mycomment.models import head_comment, comment
# Register your models here.

admin.site.register(head_comment)
admin.site.register(comment)
```
登录127.0.0.1:8000/admin，就能看到这两个model了。
### *同步数据库
```
python manage.py makemigrations
```
下面这俩是一个功能，只是版本不同
```
python manage.py migrate
python manage.py syncdb
```
### *使用shell操作model

#### *进入Django中的交互式shell来进行数据库的增删改查等操作
```
python manage.py shell
```
#### *create数据库增加操作
```
>>> Article.objects.create(title = 'Hello World', category = 'Python', content = '我们来做一个简单的数据库增加操作')
```
#### *all和get的数据库查看操作
```
>>> Article.objects.all()  #查看全部对象, 返回一个列表, 无对象返回空list,不显示内容
>>> Article.objects.get(id = 1)  #返回符合条件的对象
```
#### *update数据库修改操作
```
>>> first = Article.objects.get(id = 1)  #获取id = 1的对象
>>> first.name='....'
```
#### *delete数据库删除操作
```
>>> first.delete()
```
### [中间件](https://www.ctolib.com/topics-102842.html)
在请求阶段，调用视图之前，Django根据它在MIDDLEWARE_CLASSES中定义的顺序自上而下应用中间件。两个可用的钩子：
```
process_request()
process_view()
```
在响应阶段，调用视图后，中间件都以相反的顺序，从下往上被应用。三个可供选择的钩子：
```
process_exception() (只有当视图引发了一个异常的时候)
process_template_response() (仅用于模板响应)
process_response()
```

```
__init__(self)
process_request(self, request)
process_view(self, request, view, args, kwargs)
process_response(self, request, response)
process_exception(self, request, exception)
```

### 7.django语法
1.导入html
{% include "header.html" %}
2.  
在base.html中添加下列：
```
{% block content %}
{% endblock %}
```
在index.html中导入base.html
```
{% extends "base.html" %}
{% block content %}
...添加内容
{% endblock %}
```
3.for语句
```
{% for blog in blog_list %}
...
  <p>{{ blog.1 }}</p>
...
{% endfor %}
```
同样的还有：
```
{% if %} {% else %} {% endif %}
```

### 8.自定义filter

1.在project/app下建立新文件夹templatetags  
2.在templatetags中建立__init__.py  
3.在templatetags中新建custom_markdown.py:  
```
import markdown

from django import template
from django.template.defaultfilters import stringfilter
from django.utils.encoding import force_text
from django.utils.safestring import mark_safe

register = template.Library()  #自定义filter时必须加上

@register.filter(is_safe=True)  #注册template filter
@stringfilter  #希望字符串作为参数
def custom_markdown(value):
    return mark_safe(markdown.markdown(value,extensions = ['markdown.extensions.fenced_code', 'markdown.extensions.codehilite']))
```

4.然后在html中应用：  
首先导入custom_markdown
```  
{% load custom_markdown %}
```
然后在需要的地方：  
```
<p>{{ data|custom_markdown }}</p>
```
data是在访问网页时传入的数据，在views.py中的方法返回：
```
return render(request, 'test.html', {'data' : data1})
```

### django ajax
```

```


### 9.部署到nginx上

安装uwsgi和nginx


```
pip3 install uwsgi
sudo apt-get install nginx
```

方法2(apt安装在以后执行命令时要加上--plugin python3)：
```
sudo apt install uwsgi
```


#### 7.1.uwsgi

测试

新建一个文件test.py,内容如下：

```
def application(env, start_response):
    start_response('200 OK', [('Content-Type','text/html')])
    return [b"Hello World"]
```

然后执行

```
uwsgi --http :8001 --wsgi-file test.py

```
apt安装的要这样
```

uwsgi --need-plugin python3 --httpocket :8001 --wsgi-file test.py

```

然后进入浏览器访问127.0.0.1:8001，浏览器返回“Hello World”就没问题了

新建一个uwsgi配置文件，我命名为my_uwsgi.ini，根据实际情况填写如下内容

```
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
```

然后执行my_uwsgi.ini

```
uwsgi --ini my_uwsgi.ini (--plugin python3)
```
杀死uwsgi，进程相关的命令：
```
列出所有进程
$ ps -ef
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 Jul25 ?        00:00:00 init
root         2     1  0 Jul25 ?        00:00:00 [kthreadd/397239]
root         3     2  0 Jul25 ?        00:00:00 [khelper/397239]
root       364     1  0 Jul25 ?        00:00:00 /usr/sbin/sshd
root       394     1  0 Jul25 ?        00:00:00 /usr/bin/python /usr/bin/ssserve
nobody     396   394  0 Jul25 ?        00:00:01 /usr/bin/python /usr/bin/ssserve
nobody     397   394  0 Jul25 ?        00:00:00 /usr/bin/python /usr/bin/ssserve
root       434     1  0 Jul25 ?        00:00:00 nginx: master process /usr/local
dy         435   434  0 Jul25 ?        00:00:00 nginx: worker process      
dy         838     1  0 04:09 ?        00:00:00 /usr/local/python3/bin/uwsgi --i
dy         849   838  0 04:09 ?        00:00:00 /usr/local/python3/bin/uwsgi --i
dy         850   838  0 04:09 ?        00:00:00 /usr/local/python3/bin/uwsgi --i
root       857   364  0 04:09 ?        00:00:00 sshd: dy [priv]  
dy         859   857  0 04:10 ?        00:00:00 sshd: dy@pts/0   
dy         860   859  0 04:10 pts/0    00:00:00 -bash
dy         882   860  0 04:11 pts/0    00:00:00 ps -ef

选出uwsgi相关进程
$ ps -ef|grep uwsgi
dy         838     1  0 04:09 ?        00:00:00 /usr/local/python3/bin/uwsgi --ini my_uwsgi.ini
dy         849   838  0 04:09 ?        00:00:00 /usr/local/python3/bin/uwsgi --ini my_uwsgi.ini
dy         850   838  0 04:09 ?        00:00:00 /usr/local/python3/bin/uwsgi --ini my_uwsgi.ini
dy         884   860  0 04:11 pts/0    00:00:00 grep uwsgi

排除grep进程
$ ps -ef|grep uwsgi|grep -v grep
dy         838     1  0 04:09 ?        00:00:00 /usr/local/python3/bin/uwsgi --ini my_uwsgi.ini
dy         849   838  0 04:09 ?        00:00:00 /usr/local/python3/bin/uwsgi --ini my_uwsgi.ini
dy         850   838  0 04:09 ?        00:00:00 /usr/local/python3/bin/uwsgi --ini my_uwsgi.ini

列出第二列
$ ps -ef|grep uwsgi|grep -v grep|awk '{print $2}'
838
849
850

杀死进程
$ ps -ef|grep uwsgi|grep -v grep|awk '{print $2}'|xargs kill -9

再查看进程
$ ps -ef
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 Jul25 ?        00:00:00 init
root         2     1  0 Jul25 ?        00:00:00 [kthreadd/397239]
root         3     2  0 Jul25 ?        00:00:00 [khelper/397239]
root       364     1  0 Jul25 ?        00:00:00 /usr/sbin/sshd
root       394     1  0 Jul25 ?        00:00:00 /usr/bin/python /usr/bin/ssserver -s ::0 -p 443 -
nobody     396   394  0 Jul25 ?        00:00:01 /usr/bin/python /usr/bin/ssserver -s ::0 -p 443 -
nobody     397   394  0 Jul25 ?        00:00:00 /usr/bin/python /usr/bin/ssserver -s ::0 -p 443 -
root       434     1  0 Jul25 ?        00:00:00 nginx: master process /usr/local/nginx/sbin/nginx
dy         435   434  0 Jul25 ?        00:00:00 nginx: worker process      
root       857   364  0 04:09 ?        00:00:00 sshd: dy [priv]  
dy         859   857  0 04:10 ?        00:00:00 sshd: dy@pts/0   
dy         860   859  0 04:10 pts/0    00:00:00 -bash
dy         908   860  0 04:16 pts/0    00:00:00 ps -ef

```
#### 7.2.nginx

更改/etc/nginx/nginx.conf

```
user dy;(注意这里！注意这里！注意这里！不然nginx没有权限读取static)

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
```

然后，重启或重载文件

```
#ubuntu
>>> service nginx restart
>>> service nginx reload
#archlinux
>>> systemctl start nginx
>>> systemctl stop nginx
或者：
sudo nginx -s stop
sudo nginx -c nginx.conf(sudo nginx)

```

这样只能在本机访问，如果要让其他人访问，需要注释server中的这一行：

```
#listen 127.0.0.1:80;    #监听本机80端口
```



### 10.设置自启动

#### 1.配置 Nginx 开启启动
如果是源码安装（比如centos下只能源码安装）
```
sudo vim /etc/init.d/nginx
```
写入下列内容
```
#!/bin/bash
# chkconfig: - 85 15
nginx=/usr/local/nginx/sbin/nginx
conf=/usr/local/nginx/conf/nginx.conf

case $1 in      #判断相应的操作(start, stop, test, reload, restart, show )
    start)
        echo -n "Starting Nginx"
        $nginx -c $conf
        echo " done"
    ;;

    stop)
        echo -n "Stopping Nginx"
        $nginx -s stop
        echo " done"
    ;;

    test)
        $nginx -t -c $conf
    ;;

    reload)
        echo -n "Reloading Nginx"
        $nginx -s reload
        echo " done"
    ;;

    restart)
        $0 stop
        $0 start
    ;;

    show)
        ps -aux|grep nginx
    ;;

    *)
        echo -n "Usage: $0 {start|restart|reload|stop|test|show}"
    ;;

esac
```
给脚本添加可执行属性 sudo chmod +x /etc/init.d/nginx

添加为系统服务（开机自启动）

sudo chkconfig --add nginx
sudo chkconfig nginx on
启动 Nginx 服务

sudo service nginx start

#### 2.配置 uWSGI 开机启动

和 nginx 类似的操作：

创建脚本

sudo vim /etc/init.d/uwsgi

写入

#!/bin/bash
# chkconfig: - 85 15
uwsgi=/usr/local/python3/bin/uwsgi
hello_conf=/home/dy/my_uwsgi.ini

case $1 in
    start)
        echo -n "Starting uWsgi"
        nohup $uwsgi $hello_conf &
        echo " done"
    ;;

    stop)
        echo -n "Stopping uWsgi"
        killall -9 uwsgi
        echo " done"
    ;;

    restart)
        $0 stop
        $0 start
    ;;

    show)
        ps -ef|grep uwsgi|grep -v grep
    ;;

    *)
        echo -n "Usage: $0 {start|restart|stop|show}"
    ;;

esac
添加可执行属性

sudo chmod +x /etc/init.d/uwsgi
添加为系统服务

sudo chkconfig --add uwsgi
sudo chkconfig uwsgi on
启动 uWSGI 服务

sudo service uwsgi start
注：脚本开头要写上 # chkconfig: - 85 15 ，不然无法添加为系统服务（网上有的代码少了这句话）。
