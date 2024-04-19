## Resources

- [《Security Engineering》](https://www.cl.cam.ac.uk/~rja14/book.html)

## Project: superlists

### Required Software Installations

1. The Firefox web browser
2. Geckodriver
   - [Download page](https://github.com/mozilla/geckodriver/releases)
   - Move the file to `~/.local/bin`
   - `chmod +x /usr/local/bin/geckodriver`
   - Add the path to `.bashrc` or .`zshrc`
   - Check `geckodriver --version`
3. Git
4. A `virtualenv` with `Python 3`, `Django`, and `Selenium` in it

### Activating and Deactivating the Virtualenv

- `mkvirtualenv superlists` 创建
- `deactivate` 退出
- `workon superlists` 进入

### Getting Django Up and Running

- 添加在最外层
- `django-admin startproject superlists`
- 进入第一层创建的`superlists`文件夹
- `python3 manage.py runserver` 启用 server

### functional_tests.py

- 从用户的角度去写 tests
- 添加在最外层`superlists`文件夹中
- 运行方式 `python3 functional_tests.py`

### Django App

- Django encourages you to structure your code into apps
- `python3 manage.py startapp lists`，这会创建一个叫`lists`的文件夹，在其中的`tests.py`写 tests
- 在`settings.py`中将`"lists"`添加到`INSTALLED_APPS`中
- 测试时用`python3 manage.py test`

### Unit Tests

- 从程序员的角度去写 tests
- `resolve` is the function Django uses internally to resolve URLs and find what view function they should map to. 例如把`resolve('/')`的结果和某个具体的 view 去作比较（views 写在`lists/views.py`里）
- 用`urls.py`将 urls 和 view 链接起来。

### 循环以下命令

- python manage.py runserver
- python functional_tests.py
- python manage.py test

### Refactoring

- refactor 的时候不要添加任何新功能，只是改变结构，把一个文件里的内容写到新的地方而已。

### Templates

- 在`lists`文件夹里创建新的文件夹`templates`以存储 html 文件
- 在`views.py`文件中`render(request, 'home.html')`

### The Django Test Client

- Django gives us a tool called the Django Test Client, which has built-in ways of checking what templates are used

```python
response = self.client.get('/')
self.assertTemplateUsed(response, 'home.html')
```

### Django’s CSRF protection

```html
<form method="post">
  {% csrf_token %}
  <!-- 表单的其他部分 -->
</form>
```

### Associate input tag with request.POST

- Include a `name=` attribute in the `input` tag

### Red/Green/Refactor and Triangulation

The unit-test/code cycle is sometimes taught as Red, Green, Refactor:

- Start by writing a unit test which fails (Red).
- Write the simplest possible code to get it to pass (Green), even if that means cheating.
- Refactor to get to better code that makes more sense.

### Django Object-Relational Mapper (ORM) and Model

- ORM 类似一个 database，具体功能写在`lists.models`里
- 可以写如下的 unit_test

```python
from lists.models import Item
...
def test_saving_and_retrieving_items(self):
   # Create an item
   first_item = Item()
   first_item.text = 'xxx'
   first_item.save()

   # Bundle the items
   saved_items = Item.objects.all()

   # 获取item，将saved_items作为一个array对待
   first_saved_item = saved_items[0]
   self.assertEqual(first_saved_item.text, 'xxx')
```

- `lists/models.py`文件中创建`Item` class

```python
from django.db import models
class Item(models.Model):
       text = models.TextField(default='') # 自定义的item method
```

- 要通过 Database Migration 才能成功创建这个 model Item。每次改变 models 中的数据结构都要重新使用命令`python3 manage.py makemigrations`。运行`python manage.py migrate`来实际更新数据库结构。此命令会查找所有还未应用的迁移文件，并按照依赖关系顺序执行它们，更新数据库结构。
- 手动清除 database 的数据

```bash
rm db.sqlite3
python3 manage.py migrate --noinput
```

### 迁移 functional tests

- 创建文件夹`functioanl_tests`
- 在该文件夹中创建文件`__init__.py`。
- 用`git mv functional_tests.py functional_tests/tests.py`将原本的文件移到这个文件夹中并命名为`tests.py`
- 现在要用`python3 manage.py test functional_tests`来运行这个文件夹中的 tests。并且这样做就可以将原本文件底部的`if __name__ == '__main__'`移除

### LiveServerTestCase

- 这个 Django 的 method 可以帮助我们自动管理，即创建和清除 database
- `self.browser.get('http://localhost:8000')`改成`self.browser.get(self.live_server_url)`

### <form action="">

- 在 HTML 中，<form>标签用于创建一个表单，用于用户输入数据。action 属性是<form>标签的一个重要属性，它定义了当表单提交时，数据应该发送到服务器上的哪个 URL。换句话说，action 属性指定了处理表单数据的服务器端脚本的位置。

### 迁移 urls

- For URLs that only apply to the lists app, Django encourages us to use a separate lists/urls.py, to make the app more self-contained. The simplest way to make one is to use a copy of the existing urls.py: `$ cp superlists/urls.py lists/`

### 安装 bootstrap

- 在`lists`文件夹中创建新的文件夹`static`, 将下载下来并解压后的`bootstrap dist`文件夹移到里边，重新命名文件夹为`bootstrap`

```zsh

$ mkdir lists/static
$ mv bootstrap-3.3.4-dist lists/static/bootstrap $ rm bootstrap.zip
```

### Template inheritance

- `<h1>{% block header_text %}{% endblock %}</h1>` 用 block 来标记等会可以被 child templates 替换的区域。相当于一个 skeleton，这样只要 child html 中向这些 block 填不同内容就可以渲染出不同页面。

### Functional test 中将`LiveServerTestCase`切换成`StaticLiveServerTestCase`

- 简单说前者比后者功能更齐全。如果你的 project 中用到了 bootstrap 中的 css，js 这些 static file，那么用前者才能调用这些文件。
