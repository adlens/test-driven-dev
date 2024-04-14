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

- 要通过 Database Migration 才能成功创建这个 model Item。使用命令`python3 manage.py makemigrations`。之后如果要改变这些 models 也要重新运行`python manage.py migrate`
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
