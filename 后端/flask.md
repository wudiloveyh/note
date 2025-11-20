# flask

> flask是python的一个轻量级的web框架

优点：代码少，上手快，可读性高

## 环境准备

- 需要python3.8以上

- pip

- 文本编辑器：VScode

## 创建flask项目

创建虚拟环境

```bash
python -m venv venv 
```

![image-20251120162728894](https://cdn.jsdelivr.net/gh/wudiloveyh/my-image-bucket@main/images/20251120162728962.png)

进入虚拟项目环境

```bash
.\venv\Scripts\Activate.ps1
# 验证是否进入
Get-Command python
# 如果进入了的话python的路径就是当前虚拟环境的路径
```

安装flask

```bash
pip install flask
```

创建一个最简单的flask程序（app.py）：

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def index():
    return "Hello Flask!"

if __name__ == "__main__":
    app.run(debug=True)
```

运行

```bash
python app.py 
```

![image-20251120163759884](https://cdn.jsdelivr.net/gh/wudiloveyh/my-image-bucket@main/images/20251120163759926.png)

访问127.0.0.1:5000就可以了

![image-20251120163828634](https://cdn.jsdelivr.net/gh/wudiloveyh/my-image-bucket@main/images/20251120163828671.png)

以上就是简单的的flask项目了

## 进阶

app.py:

```python
from flask import Flask

app = Flask(__name__)

# 主页
@app.route("/")
def index():
    return "Hello Flask!"

# 第二个页面
@app.route("/about")
def about():
    return "这是关于页面"

# 返回 HTML 页面
@app.route("/html")
def html_page():
    return """
    <h1>这是一个 HTML 页面</h1>
    <p>你已经成功掌握 Flask 返回 HTML 的方法！</p>
    """

if __name__ == "__main__":
    app.run(debug=True)
```

![image-20251120164040907](https://cdn.jsdelivr.net/gh/wudiloveyh/my-image-bucket@main/images/20251120164040935.png)

![image-20251120164055049](https://cdn.jsdelivr.net/gh/wudiloveyh/my-image-bucket@main/images/20251120164055083.png)

### 模板

项目结构

flask默认会从项目中的templates文件夹中读取html页面

```bash
flask_demo/
    app.py
    templates/
        index.html
        about.html
```

创建index文件

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>首页</title>
</head>
<body>
    <h1>欢迎来到 Flask 网站！</h1>
    <p>这是使用模板返回的页面。</p>
    <p>传给模板的变量 name = {{ name }}</p>
</body>
</html>
```

创建about文件

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>关于</title>
</head>
<body>
    <h1>关于页面</h1>
    <p>作者：{{ author }}</p>

    <h2>兴趣列表：</h2>
    <ul>
        {% for item in hobbies %}
        <li>{{ item }}</li>
        {% endfor %}
    </ul>
</body>
</html>
```

### 表单和请求处理

项目结构：

```bash
flask_demo/
    app.py
    templates/
        index.html
        about.html
        form.html
```

form文件：

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>提交表单</title>
</head>
<body>
    <h1>简单的表单示例</h1>

    <form action="/submit" method="POST">
        <label>请输入你的名字：</label>
        <input type="text" name="username" required>

        <button type="submit">提交</button>
    </form>
</body>
</html>
```

修改app.py

```python
from flask import Flask, render_template, request

app = Flask(__name__)

@app.route("/")
def index():
    return render_template("index.html", name="初雪")

@app.route("/about")
def about():
    return render_template(
        "about.html",
        author="初雪",
        hobbies=["编程", "打MC", "搞服务器", "研究网络"]
    )

# 显示表单页
@app.route("/form")
def form():
    return render_template("form.html")

# 接收 POST
@app.route("/submit", methods=["POST"])
def submit():
    username = request.form.get("username")
    return f"你好，{username}！表单已收到！"

if __name__ == "__main__":
    app.run(debug=True)
```

### 文件上传

增加模板：

```bash
templates/
    upload.html
```

内容

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>文件上传</title>
</head>
<body>
    <h1>上传文件</h1>

    <form action="/upload" method="POST" enctype="multipart/form-data">
        <input type="file" name="file" required>
        <button type="submit">上传</button>
    </form>
</body>
</html>
```

新建文件夹保存

```bash
flask_demo/
    uploads/
```

更新app.py

```python
from flask import Flask, render_template, request, redirect, url_for
import os

app = Flask(__name__)

# 设置上传文件保存的目录
UPLOAD_FOLDER = "uploads"
app.config['UPLOAD_FOLDER'] = UPLOAD_FOLDER


@app.route("/upload")
def upload_page():
    return render_template("upload.html")


@app.route("/upload", methods=["POST"])
def upload_file():
    file = request.files.get("file")

    if not file:
        return "没有收到文件"

    # 保存文件
    filepath = os.path.join(app.config['UPLOAD_FOLDER'], file.filename)
    file.save(filepath)

    # 上传完成后跳转到一个成功页面
    return redirect(url_for("upload_success", filename=file.filename))


@app.route("/success/<filename>")
def upload_success(filename):
    return f"文件 {filename} 上传成功！已保存到服务器。"
```

### 连接数据库

安装扩展

```bash
pip install flask_sqlalchemy
```

项目结构

```bash
flask_demo/
    app.py
    templates/
    uploads/
    test.db     ← 运行后自动生成
```

更新app.py文件

```python
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)

# 配置 SQLite 数据库
app.config["SQLALCHEMY_DATABASE_URI"] = "sqlite:///test.db"
app.config["SQLALCHEMY_TRACK_MODIFICATIONS"] = False

db = SQLAlchemy(app)

# ----------- 定义数据库模型（等于定义一张表）--------------
class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)  # 主键
    name = db.Column(db.String(50), nullable=False)
    age = db.Column(db.Integer)

    def __repr__(self):
        return f"<User {self.name}>"
```

这里用sqlite，个人觉得小项目用sqlite就可以了

创建数据库

在 app.py 最下面（`if __name__ == "__main__"` 上面）加一行：

```python
with app.app_context():
    db.create_all()
```

![image-20251120170142051](https://cdn.jsdelivr.net/gh/wudiloveyh/my-image-bucket@main/images/20251120170142098.png)

添加用户、查看用户

```python
@app.route("/add_user")
def add_user():
    user = User(name="初雪", age=100)
    db.session.add(user)
    db.session.commit()
    return "用户添加成功！"

@app.route("/users")
def get_users():
    users = User.query.all()
    result = "<h1>用户列表</h1>"
    for u in users:
        result += f"<p>ID: {u.id}，名字：{u.name}，年龄：{u.age}</p>"
    return result
```

修改用户

```python
@app.route("/update/<int:uid>/<int:new_age>")
def update_user(uid, new_age):
    user = User.query.get(uid)
    if not user:
        return "用户不存在"

    user.age = new_age
    db.session.commit()
    return f"已将用户 {uid} 的年龄改为 {new_age}"
```







