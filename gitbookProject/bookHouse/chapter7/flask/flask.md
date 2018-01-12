# flask
hello_world.py   Demo
```python
#coding:utf-8
import sys
reload(sys)
sys.setdefaultencoding('utf-8')

from flask import Flask
app = Flask(__name__)

# 默认的是get请求
@app.route('/')
def hello_world():
    return 'Hello world!'

# 提倡 /hello/ 不提倡 /hello
@app.route('/hello/')
def hello():
    return "hello"

# 注意:这个路由的最后不能加/
@app.route('/user/<username>')
def show_user_profile(username):
    # show the user profile for that user
    return 'User %s' % username

@app.route('/aa/bb/cc/dd/')
def hello2():
    return "aa-bb-cc-dd"

if __name__ == '__main__':
    app.run()

```

运行:
```
http://127.0.0.1:5000/
http://localhost:5000/hello/
http://localhost:5000/user/guoling
http://localhost:5000/aa/bb/cc/dd/
```

运行效果图:

```
不会用快捷键截图
```

urlFor.py Demo
```python
#coding:utf-8 
from flask import Flask,url_for
app = Flask(__name__)
 
@app.route('/')
def index():
    return 'index 页面!'
 
@app.route('/login/')
def login():
    return "login 页面"
 
@app.route('/user/<username>')
def show_user_profile(username):
    # show the user profile for that user
    return 'User %s' % username

with app.test_request_context():
    print(url_for('index'))
    print(url_for('login'))
    print(url_for('login', next='/'))
    print(url_for('show_user_profile', username='John Doe'))

if __name__ == '__main__':
    app.run()
```

运行:
~~~
http://127.0.0.1:5000/
http://127.0.0.1:5000/login/
http://127.0.0.1:5000/login/?next=guoling222
http://127.0.0.1:5000/user/guoling222
~~~

运行效果图:
~~~
不会用快捷键截图
~~~

