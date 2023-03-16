---
title: TDD
date: 2022-03-24 21:30:25
tag: Python
---

## 项目的创建

[Django3官方文档](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial01/)

##### 创建Django项目：`django-admin startproject proj_name`

```markdown
 	在当前路径下创建一个名为proj_name的文件，该文件为Django项目的根文件夹，文件夹里面也有一个proj_name，该文件为Django项目的全局配置文件，是一个python包
```



##### 运行Django项目：`python manage.py runserver 80`

```markdown
	开启一个运行在80端口的http服务，本地运行使用http://localhost即可访问
```



#### unittest模块的使用——代码示例

```python
from selenium import webdriver
import unittest
class NewVisitorTest(unittest.TestCase):	#继承于unittest.TestCase的类
    def setUp(self):					#类似于c++的构造函数，在该类实例化时调用
        self.browser = webdriver.Chrome();
        self.browser.implicitly_wait(3)		#隐式等待，等待3秒，让selenium有充足时间加载
    def tearDown(self):					#类似于c++的析构函数，在类的实例结束时调用
        self.browser.quit()				#关闭浏览器
    def test_can_start_a_list_and_retrieve_it_later(self): #自定义的测试函数
        self.browser.get('http://localhost:8000')
        self.assertIn('To-Do', self.browser.title)
        self.fail('Finish th test!')

if __name__ == '__main__':
    unittest.main(warnings = 'ignore')	#用unittest.main启动测试，warnings  = 'ignore'指忽略警告
```

## Django应用创建&&使用

#### 创建Django的应用：`python manage.py startapp app_name`

```markdown
	在Django项目的根目录下创建一个名为app_name的文件夹，里面包含一些测试用的占位文件，在 Django 中，每一个应用都是一个 Python 包
```

#### 启动Django的应用：`python manage.py test`

```markdown
	运行app_name下的test.py
```

### 测试

```powershell
python manage.py test		运行功能测试和单元测试
python manage.py test lists	 指定运行lists应用中的测试	django会自动去找该目录下的tests.py文件执行测试
```

## ORM——对象关系映射器（模型化数据库，models.py中的一个类就是数据库中的一张表）

```markdown
1.	在数据库中创建新记录：
	1.1	创建一个对象	如：a = A()	#A是一个类，实际上就是数据库中的一张表，一个对象就是表中的一行
	1.2 给对象的属性赋值	如：a.text = 'hello'
	1.3	调用save()函数保存到数据库中	如：a.save()则a的值都会被储存到数据库中
	1.4	objects即类属性，使用A.objects.all()取出这个表中的全部记录，得到一个类似于列表的对象，用[]取值
	1.5 count()函数，统计数据库中该类对象的个数
	1.6 get()函数，获取特定的对象，如A.objects.get(id=test_id)，获取id为test_id的那个对象
2.	ORM的类要在models.py中声明和定义，且要使用save(),count()这些函数，该类得继承于models.Model
	如：
		class test(models.Model):
			text = models.TextField()
3.	创建的类（表）默认有一个id作为主键，其它的属性（列）都需要自定义，（）中加入default=xxx设置缺省值
	3.1	models.TextField()		定义文本字段（无需限制长度）
	3.2 models.IntegerField()	定义整型字段
	3.3 models.CharField()		定义字符型字段（需要限制长度）
	3.4 models.DateField()		定义时间z
4. 通过外键使俩个类关联起来
	即如果要在A类中使用B类作为A类的一个属性则要用models.ForeignKey(B, default=None)来声明
	如：
		class B(models.Model):
			pass
		class A(models.Model):
			b = models.ForeignKey(B, on_delete=models.CASCADE)
注：定义外键时on_delete必须给值

5.	models中on_delete的值含义
    	on_delete=None, # 删除关联表中的数据时,当前表与其关联的field的行为
    	on_delete=models.CASCADE, # 删除关联数据,与之关联也删除
    	on_delete=models.DO_NOTHING, # 删除关联数据,什么也不做
    	on_delete=models.PROTECT, # 删除关联数据,引发错误ProtectedError
    # ForeignKey('关联表', on_delete=models.SET_NULL, blank=True, null=True)
    	on_delete=models.SET_NULL, # 删除关联数据,与之关联的值设置为null（前提FK字段需要设置为可空,一对一同理）
    # ForeignKey('关联表', on_delete=models.SET_DEFAULT, default='默认值')
    	on_delete=models.SET_DEFAULT, # 删除关联数据,与之关联的值设置为默认值（前提FK字段需要设置默认值,一对一同理）
    	on_delete=models.SET, # 删除关联数据,
    		a.关联的值设置为指定值,设置：models.SET(值)
    		b.关联的值设置为可执行对象的返回值,设置：models.SET(可执行对象)
```

## migration——根据models.py创建数据库

```markdown
1.	python manage.py makemigrations #后面可加app_name
	为改动创建迁移记录
2.	python manage.py migrate
	将操作同步到数据库 				#建议同步前先把之前的文件删除掉
2.	查看迁移文件
	迁移文件都保存在migrations文件夹中
```



## URL解析与正则表达式：

### 基础概念

```markdown
1.	使用resolve函数解析URL，
注：resolve函数Django2.0后将django.core.urlresolve包更名为了django.urls

2.	django中如果url几乎正确，但最后缺少一个 / 则会永久重定向

3.	在项目根目录下的项目名目录下中的urls.py中包含整个项目所有URL解析和对应的调用函数
如：👇
```

``` python
from django.contrib import admin
from django.conf.urls import url
from lists import views as lists_view

urlpatterns = [		#要解析的URL写在这个urlpatterns列表中
    url(r'^$', lists_view.home_page, name='home_page'),	# url第一个参数是对应解析的URL，第二个参数是用于解析的函数，第三个name是这个解析的别名
    url(r'^lists/(.+)/$', lists_view.view_list, name='view_list'),  # 使用正则表达式去匹配 (.+) 是捕获组可以匹配/后的任意个字符，捕获到的文本会作为参数传给（视图层）解析函数，传参自带一个request
    url(r'^lists/new$', lists_view.new_list, name='new_list'),
]
```



## 静态文件的放置

```markdown
可以统一放在应用的static文件夹下，在Django的根目录下的项目名文件夹下的setting文件中的STATIC_URL = '/static/'
指定了Django会在每一个的应用的目录中寻找static文件
静态文件使用方法：/static/文件在static文件夹下的路径
```

