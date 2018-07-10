---
title: Python系列教程之学生信息管理系统
date: 2018-04-23 09:41:47
categories: "开发"
tags:
	- 技术
	- 编程语言
	- Python

---

![Python系列教程之学生信息管理系统][Python]

**1、上传图片/文件等资源**

有时候需要添加一些附件，例如，新生刚入学，大家相互之间还不熟悉，希望能通过照片来加深印象，并且方便教学管理。

首先，对demo/urls.py文件进行改造，给urlpatterns添加static(settings.MEDIA\_URL, document\_root=settings.MEDIA\_ROOT)：

``````````
urlpatterns = [ path(r'', xadmin.site.urls),] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
``````````

然后在demo/settings.py文件中添加

\# 指定上传位置

``````````
LOCATION = os.path.join('/', 'Users', 'babybus')
``````````

``````````
# 媒体文件根目录MEDIA_ROOT = os.path.join(LOCATION, 'Media')ROOT_URL = '/'MEDIA_URL = '/media/'
``````````

上传图片涉及到路径的获取，在models.py文件中导入os模块

``````````
import os
``````````

在models.py文件的Students类中添加一个路径获取的方法及models.ImageField字段：

``````````
def get_photo(self, filename): return os.path.join('photo', '%s_%s_%s_%s' % (self.class_name, self.name, self.id, os.path.splitext(filename)[1]))
``````````

``````````
photo = models.ImageField(verbose_name='照片', upload_to=get_photo, blank=True, null=True)
``````````

同时同步一下数据库操作，这样页面就多出一个照片上传的选项了，选择好照片并保存：

![Python系列教程之学生信息管理系统][Python 1]

![Python系列教程之学生信息管理系统][Python 2]

**2、筛选、过滤、排序**

**1）筛选**

今年学校的录取率爆满，生源特别好，要在一个数据库中找到对应的学生，必然需要用到搜索功能。

修改adminx.py文件，在StudentsAdmin类中添加:

``````````
search_fields = ('name', )
``````````

![Python系列教程之学生信息管理系统][Python 3]

要是我们想通过班级或者学科来查找这个班级或者选修这门学科的所有学生，方法还会是一样的吗？我们先试试：

``````````
search_fields = ('name', 'class_name', 'subjects',)
``````````

结果，报错了：

![Python系列教程之学生信息管理系统][Python 4]

这是怎么回事呢？原来，我们搜的“班级”和“学科”这两个字段一个是外键一个是含有多对多关系，Student模型中的这两个字段名称并不是其实际名称，要在字段后加“\_\_”两个下划线，然后再添加外键或多对多关系实际的字段名：

``````````
search_fields = ('name', 'class_name__class_name', 'subjects__name',)
``````````

![Python系列教程之学生信息管理系统][Python 5]

现在妥妥的了。

![Python系列教程之学生信息管理系统][Python 6]

**2）过滤：**

如果只想查看学生表中的男生或者女生的信息，那就用到了过滤功能：

修改adminx.py文件，在StudentsAdmin类中添加：

``````````
list_filter = ('sex',)
``````````

![Python系列教程之学生信息管理系统][Python 7]

**3）排序：**

如果想让学生按某字段的顺序来排序，同样我们需要在adminx.py文件中的StudentsAdmin类中添加ordering选项：

\# 顺序排序

ordering = ('age', 'name', )

\# 逆序排序，在前面加一个减号"-"，例如按年龄倒序排列

ordering = ('-age',)

这表示同时按照年龄和姓名字段来排序。

![Python系列教程之学生信息管理系统][Python 8]

**二、定制网站信息**

我们希望登录网站的时候，显示站点的名称，修改adminx.py文件，添加LoginViewAdmin类，并注册：

``````````
from xadmin.views.website import LoginViewclass LoginViewAdmin(LoginView): title = '学生信息管理系统'xadmin.site.register(LoginView, LoginViewAdmin)
``````````

![Python系列教程之学生信息管理系统][Python 9]

还可以继续修改，例如浏览器标题和左上角的网页标题以及页脚的版权信息：

``````````
from xadmin.views import CommAdminViewclass GlobalSetting(CommAdminView): # 左上角及浏览器标题site_title = '学生信息管理系统'# 页脚版权信息site_footer = 'Copyright © 2018 宝宝巴士'xadmin.site.register(CommAdminView, GlobalSetting)
``````````

![Python系列教程之学生信息管理系统][Python 10]

左侧边栏如果以后项目越来越多了，有一个归类会更好看些，也方便管理操作。这就需要在GlobalSetting类中添加

``````````
menu_style = 'accordion'
``````````

![Python系列教程之学生信息管理系统][Python 11]


[Python]: http://p3.pstatp.com/large/pgc-image/152444763544336eff82e9a
[Python 1]: http://p3.pstatp.com/large/pgc-image/1524447592814061ab83098
[Python 2]: http://p3.pstatp.com/large/pgc-image/1524447658185b59a35ca9b
[Python 3]: http://p9.pstatp.com/large/pgc-image/1524447593088f29bff95ee
[Python 4]: http://p9.pstatp.com/large/pgc-image/15244475927571b8fa05581
[Python 5]: http://p3.pstatp.com/large/pgc-image/152444759278676d475e32f
[Python 6]: http://p9.pstatp.com/large/pgc-image/15244476724680f3efd5d6f
[Python 7]: http://p3.pstatp.com/large/pgc-image/1524447592758f8c6a8111c
[Python 8]: http://p1.pstatp.com/large/pgc-image/1524447686853473a868f0a
[Python 9]: http://p3.pstatp.com/large/pgc-image/1524447592699405aede22c
[Python 10]: http://p9.pstatp.com/large/pgc-image/1524447592733cab5d41dc2
[Python 11]: http://p3.pstatp.com/large/pgc-image/1524447592749b6a8817103
 *  **原文作者：** 空上雪如烟
 *  **原文链接：** https://www.toutiao.com/item/6547453046807331332/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。