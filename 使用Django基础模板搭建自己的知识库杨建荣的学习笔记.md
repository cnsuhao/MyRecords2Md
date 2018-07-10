---
title: 使用Django基础模板搭建自己的知识库
date: 2017-12-15 10:24:13
categories: "开发"
tags:
	- Django
	- 编程语言
	- Windows
	- 数据挖掘
	- Python

---

今天给自己定了个小目标，一定要先做出点东西来，要不别回家了，哈哈。

当然我可不是瞎说，做事得有计划和目标。

我分为了两个阶段，计划在一天内完成，上午是阶段一，下午是阶段二。第一阶段全部要完成，第二阶段满足60分算是达标。任务和细节如下：

![使用Django基础模板搭建自己的知识库][Django]

首先，上来就是一个大工程，难免也吃不消，而且很难见到效果，有什么好的小项目呢，我转眼一想，先拯救下自己吧。

我每天要看不少的文章，有些是碰到问题之后再去看，有些是针对性的去查看，看到好的文章就收藏了，结果发现收藏的越多，越是难以利用起来，因为太多了，管理起来也不方便，大家知道浏览器的收藏夹，其实简单用还行，做管理还是很不方便的，至少没有搜索功能。如果想搜索哪些时间段搜索了哪些网页，把链接都保留下来，这样我就可以放心的关掉浏览器了，殊不知，这些天我的浏览器打开了快40个页面，还舍不得关掉。所以先解决我的问题，做一个本地的知识库，随时可以用。

所以第一阶段我就在windows上来做，也没打算用MySQL，自带的sqlite足够了。而且我本机要用的话，随时启动python即可。为了快速迭代实现功能，我准备使用自带的admin模板来做，刚好满足需求，而且页面还看起来简洁美观。

这是初步做成的效果图，会在这个基础上逐步完善。

![使用Django基础模板搭建自己的知识库][Django 1]

第一阶段的工作很快开始了，配置环境，简单捋一捋。

先得到django的版本

python -c "import django; print(django.get\_version())"

然后创建项目kmp(knowledge management portal)和应用kmpapp

django-admin startproject kmp

cd kmp

django-admin startapp kmpapp

开启web服务迅速验证

python manage.py runserver \[IP:PORT\]

如果能看到一个欢迎界面，证明就达标了。

然后修改settings.py文件，添加应用

INSTALLED\_APPS = (

'django.contrib.admin',

'django.contrib.auth',

'django.contrib.contenttypes',

'django.contrib.sessions',

'django.contrib.messages',

'django.contrib.staticfiles',

'kmpapp',

)

修改支持语言为中文

LANGUAGE\_CODE = 'zh-Hans'

注释掉下面的一行。

'django.middleware.csrf.CsrfViewMiddleware',

然后修改kmpapp/urls.py，添加下面的内容

url(r'^kmpapp/index/$','kmpapp.views.index')

修改kmpapp/views.py，添加如下的内容：

from django.http import HttpResponse

def index(req):

return HttpResponse('Hello World')

然后再次验证 python manage.py runserver \[IP:PORT\]

如果能看到hello world，继续往下走。

我们配置models.py文件，主要的思想就是创建3个类，一个是一级目录（parent\_category）,一个二级目录(child\_category),一个url表（url\_info）

其中二级目录表和url表是有外键的。

models.py的文件内容如下：

``````````
from django.db import models# Create your models here.class km_parent_category(models.Model):category_pid = models.AutoField(primary_key=True)category_name = models.CharField(max_length=200)category_memo = models.CharField(max_length=200)class Meta:db_table = 'km_parent_category'verbose_name = 'CATEGORY_PARENT'verbose_name_plural = 'CATEGORY_PARENT'ordering = ['category_pid']def __unicode__(self):return '%s %s' % (self.category_pid, self.category_name)class km_child_category(models.Model):category_cid = models.AutoField(primary_key=True)category_name = models.CharField(max_length=200)category_memo = models.CharField(max_length=200)#category_parent_pid = models.ForeignKey( 'km_parent_category', related_name='category_parent_pid', to_field='category_pid', verbose_name='Url_category')category_parent_pid = models.ForeignKey( 'km_parent_category')class Meta:db_table = 'km_child_category'verbose_name = 'CATEGORY_CHILD'verbose_name_plural = 'CATEGORY_CHILD'ordering = ['category_cid']def __unicode__(self):return '%s %s' % (self.category_cid, self.category_name)class km_url_info(models.Model):url_id = models.AutoField(primary_key=True)url_title = models.CharField(max_length=100)url_detail = models.CharField(max_length=200)create_date = models.DateTimeField('date created')url_memo = models.CharField(max_length=200)URL_STATUS = ((1, 'VALID'),(0, 'INVALID'))url_status = models.IntegerField(choices=URL_STATUS)category_id = models.ForeignKey('km_child_category', related_name='category_id', to_field='category_cid', verbose_name='Url_category')#category_id = models.ForeignKey('km_child_category')class Meta:db_table = 'km_url_info'verbose_name = 'URL_INFO'verbose_name_plural = 'URL_INFO'ordering = ['url_id']def __unicode__(self):return '%s %s' % (self.url_id, self.url_title)
``````````

views.py的文件内容如下：

``````````
from django.shortcuts import renderfrom kmpapp.models import km_url_info# Create your views here.from django.http import HttpResponsefrom kmpapp.models import km_parent_categoryfrom kmpapp.models import km_child_categoryfrom kmpapp.models import km_url_infofrom django.shortcuts import render_to_responsedef index(req):return HttpResponse('Hello World')def add_url(req,name):km_parent_category.objects.create(category_name=name)return HttpResponse('OK')def getList(request):category = km_parent_category.objects.all()url_info = km_url_info.objects.all()child_info = km_child_category.objects.all()result = render_to_response('test.html', {'km_category': category, 'km_url_info': url_info, 'km_child_info': child_info})return result
``````````

admin.py的文件内容如下：

``````````
from django.contrib import admin# Register your models here.from kmpapp.models import km_parent_categoryfrom kmpapp.models import km_child_categoryfrom kmpapp.models import km_url_infoclass category_parent_admin(admin.ModelAdmin):fields = ['category_name', 'category_memo']list_display = ('category_name','category_memo')admin.site.register(km_parent_category, category_parent_admin)class category_child_admin(admin.ModelAdmin):fields = [ 'category_parent_pid','category_name', 'category_memo']list_display = ( 'category_parent_pid', 'category_name','category_memo')admin.site.register(km_child_category, category_child_admin)class url_admin(admin.ModelAdmin):fields = ['category_id', 'url_title', 'url_detail', 'create_date', 'url_memo', 'url_status']list_display = ('category_id', 'url_title', 'url_detail', 'create_date', 'url_memo', 'url_status')admin.site.register(km_url_info, url_admin)
``````````

使用如下的方式生成数据表。

python manage.py makemigrations kmpapp

python manage.py sqlmigrate kmpapp 0001

python manage.py migrate

创建管理用户

python manage.py createsuperuser

再次验证即可。

python manage.py runserver \[IP:PORT\]

可以很方便的修改url的信息，至少对我来说，我可以很快完成这些力所能及的工作。

![使用Django基础模板搭建自己的知识库][Django 2]

第一阶段的工作比预期晚了一个小时，第二阶段的工作是在测试的虚拟机上做的，碰到了一个奇怪的问题，怎么调试都不对，一直调试到晚上8:30，回到家都快10点，吃了点东西继续调，我感觉是Django的一个bug,自动转换的form表单总是有点问题，直到11:30的时候才算搞好。

总算和完成了这些基础的工作，第二阶段的任务可以勉强打60分。剩下的事情就有搞头了。


[Django]: /pro/os/crawler/IQV6-ZE6B-IFZR.jpg
[Django 1]: /pro/os/crawler/FZBF-QERR-RM6J.jpg
[Django 2]: /pro/os/crawler/ANUA-2EYM-BBEJ.jpg
 *  **原文作者：** 杨建荣的学习笔记
 *  **原文链接：** https://www.toutiao.com/item/6499593993280553485/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。