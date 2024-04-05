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

- 添加在最外层
- 运行方式 `python3 functional_tests.py`
