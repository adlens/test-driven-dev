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

```
   response = self.client.get('/')
   self.assertTemplateUsed(response, 'home.html')
```
