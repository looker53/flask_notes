Flask 教程

相关资料

- [思诚之道](http://www.bjhee.com/)
- [Flask之旅](https://spacewander.github.io/explore-flask-zh/)
- [flask源码解析](https://cizixs.com/2017/01/10/flask-insight-introduction/)



## 1、Flask 快速开始

Flask 项目的一般架构：

![img](https://ky8tjff7tg.feishu.cn/space/api/box/stream/download/asynccode/?code=NzNjNmFjMjUxYmE1Njk3YjIxZmMzZjY0MmIyNjNmMmVfdHQ4bGpiVWpmYmtxWjNRemU0UGtOVXFSMzcycUVSNzVfVG9rZW46Ym94Y25oMTZiM1B4THdXb29zcHlxWkZkSFdjXzE2MTcxMDYyNzI6MTYxNzEwOTg3Ml9WNA)







### 1.1、适用项目

- 中小型项目
- 非纯web项目
- 学习

例子1： unsplash

例子2： picom

### 1.2、wsgi 协议

WSGI: web server gateway interface, Web服务器和Web应用程序或框架之间的简单和通用接口. 

- 给webserver 一个可以调用的对象，函数、方法、类或者实现了 call 的对象。
- 对象接收 env 和 make_response 参数



![img](https://ky8tjff7tg.feishu.cn/space/api/box/stream/download/asynccode/?code=ZTRiMjRhYTU3NTM1YjYyNjA2MjQ0MGQxZDMwYWM1ZmZfZDFnU2JnT21jMFFZckZBeE8waGpKbEtxTTBCTFVEQ0dfVG9rZW46Ym94Y25GRm50Y1ZUUVJCT1huS1kwT3l3R3pnXzE2MTcxMDYyNzI6MTYxNzEwOTg3Ml9WNA)



**静态服务器**

![img](https://ky8tjff7tg.feishu.cn/space/api/box/stream/download/asynccode/?code=YjQyN2Y3M2UwNjhjZmEzMGU2MTQwZjk4NzFmMGQ4ZGVfTkpoWFVaU2xad3ZmdW5NOVNNUVNoVTlZMWQ1aFhvaldfVG9rZW46Ym94Y25CZVhuM2VEeEp3eWQ1T1BDUmdMU3lmXzE2MTcxMDYyNzI6MTYxNzEwOTg3Ml9WNA)



**动态服务器**

![img](https://ky8tjff7tg.feishu.cn/space/api/box/stream/download/asynccode/?code=YTJhMThmZThkM2VjZjk0MTM4ZDkzYjVjMDhlMTIxOTNfMWNPT1JtVUpDY2o3ZTE2TkJUVWZCMnVXM0JFdVE4bXBfVG9rZW46Ym94Y25Celg3UFBpOWQ4Uk9BWE1HZmx4dlliXzE2MTcxMDYyNzI6MTYxNzEwOTg3Ml9WNA)



**wsgi代码**

```
from wsgiref.simple_server import make_server


def listen(envrion, make_response):
    print("正在监听")
    make_response('200 OK', [('content-type','text/plain')])
    return [b'hello']


server = make_server("", 5032, listen)

server.serve_forever()
```

wsgi 规则1：

- 所有的python服务器一定要接收一个可以被调用的对象为参数，这个参数就代表 application

注意：`

1、 status 必须是字符串，不能是 200 数字，且要有 4 个字符以上。

2、 header 是一个元组构成的列表，不能是字典。（实际字典更方便啊）

3、 返回值是列表且要是 byte 类型。encode

wsgi server 的作用：

- 获取请求
- 请求分派
- 返回响应
- server - app 分离

wsgi 的好处：

- 提供了标准
- 更容易创建中间件，插件

web 框架最核心的事情：

- 请求分派（url, 请求参数，请求方法）



**响应**

web 框架就是让你更舒服的去获取请求，返回响应。

参考：

- [FullStackPython](https://www.fullstackpython.com/wsgi-servers.html)
- [WSGI Server Application Interface](https://www.toptal.com/python/pythons-wsgi-server-application-interface)
- [basic wsgi](https://www.agiliq.com/blog/2013/07/basics-wsgi/)





### 1.3、安装

- 直接安装
- 虚拟环境安装
- rqm.txt 环境管理文件

直接安装：

```
pip install flask
```

虚拟环境的使用：

```
python -m venv venv
```

rqm.txt 文件的作用：

- 在另一个环境需要初始化环境的时候只需要运行 pip install -r rqm.txt
- 能保持各个第三库的版本一致，不至于造成版本不同错误。

```
flask==1.1
```

### 1.4、创建 Flask 项目

- 手工创建
- Pycharm 专业版支持 Flask 项目构建和模板引擎的代码提示
- 项目结构

一个基础的项目结构包含：

- app 对象
- 服务器运行
- 路由。 去掉所有路由会报 404 错误。

```
from flask import Flask

app = Flask(__name__)

@app.route('/')
def root():
    return "hello"

if __name__ == "__main__":
    app.run()
```

**Flask 模式和 Django 模式**

- Flask 是没有模式的
- 只要你包含了一个 app 对象，其他的东西都是由你自己拼接的。
- 增加了灵活性，同时也增加了开发成本
- 纯 web 项目一般是用 django, Flask 适合那些web只承担一部分功能的项目。

**运行项目**

- app.run() python 脚本运行。
- flask run 命令行运行：
  - 运行的脚本必须叫 [app.py](http://app.py/) 或者 wsgi.py, (类似于 pytest 的自动查找机制)
  - 且 [app.py](http://app.py/) 或者 [wsgi.py](http://wsgi.py/) 必须为模块（要有 __init__.py)
  - 如果不叫 [app.py](http://app.py/) 或者 wsgi.py， 则必须设置 FLASK_APP 环境变量。 （在 Windows 下需要使用 set 来代替 export 。）

![img](https://ky8tjff7tg.feishu.cn/space/api/box/stream/download/asynccode/?code=ZWU0OTJkYjE5MWNiMWE2YzNlZTIzZDhkM2QyNzNlZjZfS0g0SWlXaGhLWmtZdVZBZThoUVphaldHYTVDY2JVU1lfVG9rZW46Ym94Y25zSUlaeXhPOWp4aElpR01XZ0pNTEtjXzE2MTcxMDYyNzI6MTYxNzEwOTg3Ml9WNA)





**运行参数**

- **debug， 绝对不能在生产环境使用。**
- host，设成 0.0.0.0， 其他的电脑才能访问到。
- port
- load_dotenv，和 python-dotenv 扩展配合使用。
- werkzeug.serving.run_simple 当中的可选参数。

### 1.5、添加路由

- 函数名称不能重复
- 请求方法的设置
- URL 参数

设置请求方法

```
from flask import request

@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        return do_the_login()
    else:
        return show_the_login_form()
```

设置 URL 参数

```
@app.route('/user/&lt;username&gt;')
def show_user_profile(username):
    # show the user profile for that user
    return 'User %s' % escape(username)

@app.route('/post/&lt;int:post_id&gt;')
def show_post(post_id):
    # show the post with the given id, the id is an integer
    return 'Post %d' % post_id

@app.route('/path/&lt;path:subpath&gt;')
def show_subpath(subpath):
    # show the subpath after /path/
    return 'Subpath %s' % escape(subpath)
```

指定类型进行强制转化：

![Untitled](../Article/Flask 学习笔记/p/Flask快速开始/Flask 8dfe85b37b22402a9b18754ca79444e7/Untitled Database 0dd798fc1ae44fa8a59f1b84f47c4c6f.csv)

### 1.6、请求参数

- query string
- form
- json
- file
- value

### 1.7、响应

- 返回字符串
- 返回 json
- 中文返回结果， 需要设置默认选项

```
app.config.update(JSON_AS_ASCII=False)
```

- 查看源码可以看到 flask 的默认设置
- 返回自定义状态码
- 返回自定义 headers
- 返回 HTML 文档
- 设置 cookie
- 重定向
- 错误处理

### 1.8、源码

服务器运行

使用的是 werkzeug 里的 run_simple(host, port, self, **options) 方法。最后执行的是: SharedDataMiddleware 的 call 方法。

### 1.9、实战

实现项目管理的接口

- 展示所有项目
- 展示单个项目
- 创建项目
- 修改项目
- 删除项目

动态参数：

- url
- query string

```
from flask import Flask, Response, jsonify

app = Flask(__name__)

# 配置项
app.config.update(JSON_AS_ASCII=False)

@app.route('/')
def root():
    return "home"

@app.route("/projects")
def projects_list():
    projects = [
        {"id": 1, "name": "lemon班"},
        {"id": 2, "name": "前程贷"},
    ]
    return {"data": projects}


@app.route("/projects/&lt;int:id&gt;")
def project_get(id):
    project =  {"id": 2, "name": "前程贷"}
    return {"data": project}

@app.route("/projects", methods=["POST"])
def project_create():
    project =  {"id": 2, "name": "前程贷"}
    return {"data": project}, 201

@app.route("/projects/&lt;int:id&gt;", methods=["PATCH"])
def project_edit():
    project =  {"id": 2, "name": "前程贷"}
    return {"data": project}

@app.route("/projects/&lt;int:id&gt;", methods=["DELETE"])
def project_delete(id):
    project =  {"id": 2, "name": "前程贷"}
    return {"data": project}

if __name__ == "__main__":
    app.run(debug=True, host="0.0.0.0", port=5555)
curl -X POST http://...
```



### 1.10、扩展

#### static_url_path 和 static_folder

static_url_path ： 告诉程序处理静态文件的 url 路径

```
localhost:5000/static

# static_url_path='/my_static'
localhost:5000/my_static
```

当设成 my_static 以后，你在通过 static 去找就不行了。

static_folder： 告诉程序我的静态文件放在硬盘的哪个位置了。

#### static_host

```
static_host = "192.168.70.48:13579",
matching_host = True
```

必须和mathing_host 搭配使用，不然会报错，源码断言错误。

#### subdomain 子域名（不讲）

```
app = Flask(
    __name__,
    subdomain_matching=False
)

app.config["SERVER_NAME"] = "lemon.dev:5000"


@app.route(
    "/",
    subdomain='api'
)
def login():
    return json.dumps({"hello":"a you kidding"})
```



## 2、jinja2 模板

- 不方便定位问题，测试难度大
- 不太方便协作。



### 2.1、基础使用

**返回 HTML 格式字符串**

```
@app.route('/')
def home():
    return """&lt;html style="color:red"&gt;
    Hello world&lt;/html&gt;"""
```

**返回模板**

```
@app.route('/')
def home():
    return render_template('home.html')
```

**如何传递数据**

```
@app.route('/')
def home():
    projects = [
        {"name": "project01","tester":"yuz", "created_at": datetime.now()},
        {"name": "project02","tester":"musen", "created_at": datetime.now()},
    ]
    return render_template('home.html', projects=projects)
```

**指定模板路径**

```
app = Flask(
    __name__,
    template_folder='pages'
)
```

**静态文件导入**

```
&lt;link rel="stylesheet" href="{{ url_for('static', filename='demo.css') }}"&gt;

p{
    color:red
}
```

### 2.2、模板渲染

**获取变量的属性**

```
{{ foo.bar }}
{{ foo['bar'] }}

{% set a = 'name' %}
# 如果要设置变量
```

**设置变量** **set**

```
{% set a = user.name %}
{{ a }}
```

**for 循环**

```
{% for p in projects %}
项目：{{ p.name}} : {{ p.interfaces }}
{% endfor %}
```

**if 条件**

```
{% if p.name == 'yuze' %}
项目：{{ p.name}} : {{ p.interfaces }}

{% elif p.name == 'yuze' %}

{% endfor %}
```

### 2.3、全局访问

- session, 显示用户名。
- request 
- g 请求共享数据
- config
- url_for()
- get_flashed_messages()

```
&lt;body&gt;
    {{ session }}
    {{ request }}
    {{ g }}
&lt;/body&gt;
```

**URL 反向构建**

1. 反转通常比硬编码 URL 的描述性更好。
2. 你可以只在一个地方改变 URL ，而不用到处乱找。
3. URL 创建会为你处理特殊字符的转义和 Unicode 数据，比较直观。
4. 生产的路径总是绝对路径，可以避免相对路径产生副作用。
5. 如果你的应用是放在 URL 根路径之外的地方（如在 /myapplication 中，不在 / 中）， [url_for()](https://dormousehole.readthedocs.io/en/latest/api.html#flask.url_for) 会为你妥善处理。

```
&lt;form action="{{ url_for('login') }}", method="post"&gt;
```

### 2.4、管道命令过滤器

- [内置过滤器清单](http://docs.jinkan.org/docs/jinja2/templates.html#builtin-filters)

1、字符串

```
{# 当变量未定义时，显示默认字符串，可以缩写为d #}
&lt;p&gt;{{ name | default('No name', true) }}&lt;/p&gt;

{# 单词首字母大写 #}
&lt;p&gt;{{ 'hello' | capitalize }}&lt;/p&gt;

{# 单词全小写 #}
&lt;p&gt;{{ 'XML' | lower }}&lt;/p&gt;

{# 去除字符串前后的空白字符 #}
&lt;p&gt;{{ '  hello  ' | trim }}&lt;/p&gt;

{# 字符串反转，返回"olleh" #}
&lt;p&gt;{{ 'hello' | reverse }}&lt;/p&gt;

{# 格式化输出，返回"Number is 2" #}
&lt;p&gt;{{ '%s is %d' | format("Number", 2) }}&lt;/p&gt;
{# 关闭HTML自动转义 #}
&lt;p&gt;{{ '&lt;em&gt;name&lt;/em&gt;' | safe }}&lt;/p&gt;

{% autoescape false %}
{# HTML转义，即使autoescape关了也转义，可以缩写为e #}
&lt;p&gt;{{ '&lt;em&gt;name&lt;/em&gt;' | escape }}&lt;/p&gt;
{% endautoescape %}
```

2、数字

```
{# 四舍五入取整，返回13.0 #}
&lt;p&gt;{{ 12.8888 | round }}&lt;/p&gt;

{# 向下截取到小数点后2位，返回12.88 #}
&lt;p&gt;{{ 12.8888 | round(2, 'floor') }}&lt;/p&gt;

{# 绝对值，返回12 #}
&lt;p&gt;{{ -12 | abs }}&lt;/p&gt;
```

3、列表

```
{# 取第一个元素 #}
&lt;p&gt;{{ [1,2,3,4,5] | first }}&lt;/p&gt;

{# 取最后一个元素 #}
&lt;p&gt;{{ [1,2,3,4,5] | last }}&lt;/p&gt;

{# 返回列表长度，可以写为count #}
&lt;p&gt;{{ [1,2,3,4,5] | length }}&lt;/p&gt;

{# 列表求和 #}
&lt;p&gt;{{ [1,2,3,4,5] | sum }}&lt;/p&gt;

{# 列表排序，默认为升序 #}
&lt;p&gt;{{ [3,2,1,5,4] | sort }}&lt;/p&gt;

{# 合并为字符串，返回"1 | 2 | 3 | 4 | 5" #}
&lt;p&gt;{{ [1,2,3,4,5] | join(' | ') }}&lt;/p&gt;

{# 列表中所有元素都全大写。这里可以用upper,lower，但capitalize无效 #}
&lt;p&gt;{{ ['tom','bob','ada'] | upper }}&lt;/p&gt;
```

4、 tojson

```
{{ user | tojson | safe(0.10之前) }}
```

5、注册过滤器（管道）

1、方法一：装饰器

```
def register_cumstomized_jinja():


    @app.template_filter()
    def time_to_str(dt):
        return dt.strftime('%Y/%m/%d')
```

2、方法二：更集中的

```
app.jinja_env.filters['file_format'] = file_format
```

实际上：装饰器还是去调用方法二。

3、也可以在 blueprint 中注册，这是方法名称稍微变化：

```
def register_cumstomized_jinja():
    @bp.app_template_filter()
    def time_to_str(dt):
        return dt.strftime('%Y/%m/%d')
```

### 2.5、测试

除了过滤器，所谓的“测试”也是可用的。测试可以用于对照普通表达式测试一个变量。 要测试一个变量或表达式，你要在变量后加上一个 is 以及测试的名称。例如，要得出 一个值是否定义过，你可以用 name is defined ，这会根据 name 是否定义返回 true 或 false 。

测试也可以接受参数。如果测试只接受一个参数，你可以省去括号来分组它们。例如， 下面的两个表达式做同样的事情:

```
{% if a is divisibleby 3 %}
{% if loop.index is divisibleby(3) %}

a =1 
if a is None:
```

内置测试器

```
{# 检查变量是否被定义，也可以用undefined检查是否未被定义 #}
{% if name is defined %}
    &lt;p&gt;Name is: {{ name }}&lt;/p&gt;
{% endif %}

{# 检查是否所有字符都是大写 #}
{% if name is upper %}
  &lt;h2&gt;"{{ name }}" are all upper case.&lt;/h2&gt;
{% endif %}

{# 检查变量是否为空 #}
{% if name is none %}
  &lt;h2&gt;Variable is none.&lt;/h2&gt;
{% endif %}

{# 检查变量是否为字符串，也可以用number检查是否为数值 #}
{% if name is string %}
  &lt;h2&gt;{{ name }} is a string.&lt;/h2&gt;
{% endif %}

{# 检查数值是否是偶数，也可以用odd检查是否为奇数 #}
{% if 2 is even %}
  &lt;h2&gt;Variable is an even number.&lt;/h2&gt;
{% endif %}

{# 检查变量是否可被迭代循环，也可以用sequence检查是否是序列 #}
{% if [1,2,3] is iterable %}
  &lt;h2&gt;Variable is iterable.&lt;/h2&gt;
{% endif %}

{# 检查变量是否是字典 #}
{% if {'name':'test'} is mapping %}
  &lt;h2&gt;Variable is dict.&lt;/h2&gt;
{% endif %}
```

自定义测试

```
import re
def has_number(str):
    return re.match(r'.*\d+', str)
app.add_template_test(has_number,'contain_number')
{% if name is contain_number %}
  &lt;h2&gt;"{{ name }}" contains number.&lt;/h2&gt;
{% endif %}
```

判断是否是json:

```
@app.template_test()
def is_json(data):
    try:
        json.loads(data)
        return True
    except (TypeError, JSONDecodeError):
        return False
```

带参数

```
@app.template_test('end_with')
def end_with(str, suffix):
    return str.lower().endswith(suffix.lower())
{% if name is end_with('me') %}
  &lt;h2&gt;"{{ name }}" ends with "me".&lt;/h2&gt;
{% endif %}
```

### 2.6、context_processor 环境处理器

```
@app.context_processor
def project_number():
    return {"project_num":4}
```

函数用”@app.context_processor”装饰器修饰，它是一个上下文处理器，它的作用是在模板被渲染前运行其所修饰的函数，并将函数返回的字典导入到模板上下文环境中，与模板上下文合并。

不仅可以传递变量，还可以传递函数

```
@app.context_processor
def add_funtion():
    def format_file(filename):
        split = filename.rsplit('.', 1)
        if len(split) &lt;= 1:
            return filename
        return split[1]

    def get_project(user):
        return "4 of user: {}".format(user)

    return dict(format_file=format_file, get_project=get_project)

# 模板
{{ format_file('demo.pdf') }}
{{ get_project('yuze') }}

# 动态获取数据
{{ format_file(file) }}
{{ get_project(user) }}  # 参数在 view_fun 传递。
```

注意： 返回的需要是一个字典形式供模板去获取。 传递函数会发现和过滤器的作用又所重复，但是当要操作多个变量的时候这个函数会更有优势。

### 2.7、全局函数

前面的都是在上下文环境中添加，全局函数是完完全全的一个全局的。随时都可以用的。用法和全局变量一样。

```
import re


def accept_pattern(pattern_str):
    pattern = re.compile(pattern_str, re.S)

    def search(content):
        return pattern.findall(content)

    return dict(search=search, current_pattern=pattern_str)


app.add_template_global(accept_pattern, 'accept_pattern')
```

### 2.8、继承 extend



类和对象：

函数封装：多个函数之间公用了很多参数，==》 提取参数 ==》 属性

- 多个函数 ==》 关系 ==》 方法。

不同的类和对象之间，都有一些重复的行为。 ==》 继承 ==》 父类

模板渲染 ==》 继承的

basepage.：

```
&lt;!DOCTYPE html&gt;
&lt;html lang="en"&gt;
&lt;head&gt;
    &lt;meta charset="UTF-8"&gt;
    &lt;title&gt;柠檬班 - {% block title %}{% endblock %}&lt;/title&gt;
&lt;/head&gt;

&lt;body&gt;
    你好，欢迎来到柠檬班。
    &lt;p&gt; 这里是你的信息 &lt;/p&gt;
    &lt;hr&gt;
    {% block content %}
    这里是变化的内容
    {% endblock %}
    &lt;hr&gt;
    &lt;p&gt; 页面已经结束 @ copyright&lt;/p&gt;
&lt;/body&gt;

&lt;/html&gt;
```

子模板：

```
{% extends "basepage.html" %}

{% block title %} demo-herit {% endblock %}

{% block content %} 变成新内容了。 

{% for project in projects %}
    &lt;div&gt;
        {{ project.name }}
        {{ project.created_at }}
    &lt;/div&gt;
{% endfor %}

{% endblock %}
```

如果不想重写，超继承：{{ super() }}

**include**

如果有一些重复的代码片段可以重复使用，直接导入就可以了。

```
{% include 'demo_new_content.html' %}
```

**宏 python 函数(参数)**

```
{% macro render_field(field) %}
    {{ field.label }}
    {{ field }}
{% endmacro %}
```

**消息闪现 flash**

flask 端：

```
flash("you have many projects")
```

jinja: 

```
{% set msg = get_flashed_messages() %}
{{ msg }}

# 扩展 {}
```

flash() 源码：实际是把flash 数据添加到 session 里面。 再定义个全局函数去获取。

注册得例子：

```
@app.route("/login/", methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        username = request.form.get('user')
        pwd = request.form.get('pwd')
        if not all([username, pwd]) :
            flash('请输入账号或密码')
        elif username != 'yuze' and pwd != '1234':
            flash('账号或密码错误')
    return render_template('demo_flash_msg.html')
```

jinja:

```
{% set msg = get_flashed_messages() %}
{% for m in msg %}
{{m}}
{% endfor %}
登录：
&lt;form action="/login/", method="post"&gt;
    &lt;input name="user"&gt;
    &lt;input type="password", name="pwd"&gt;
    &lt;input type="submit"&gt;
&lt;/form&gt;
```

模板和前后端分离形式的区别

**获取索引 {{ loop.index }}**

| **变量**       | **描述**                              |
| -------------- | ------------------------------------- |
| loop.index     | 当前循环迭代的次数（从 1 开始）       |
| loop.index0    | 当前循环迭代的次数（从 0 开始）       |
| loop.revindex  | 到循环结束需要迭代的次数（从 1 开始） |
| loop.revindex0 | 到循环结束需要迭代的次数（从 0 开始） |
| loop.first     | 如果是第一次迭代，为 True 。          |
| loop.last      | 如果是最后一次迭代，为 True 。        |
| loop.length    | 序列中的项目数。                      |
|                |                                       |

**使用** **-** **去除元素间的空白**

默认每个 for 元素之间会有空白，如果要去除，使用 -

```
{% for p in projects -%}
项目：{{ p.name}} : {{ p.interfaces }}
{%- endfor %}
```





## 3、路由

### 3.1、路由究竟是什么？

- 添加路由的方式很简单
- 路由是把 URL 和 需要执行的程序绑定在一起。
- 输入URL,调用对应的函数

### 3.2、wsgi 程序如何处理路由？？

```
url_map = {}

def index():
    return "hello"

def login():
    return "login"

def add_url(url, view_func):
    url_map.setdefault(url, view_func)


add_url('/', index)
add_url('/login', login)


def app(env, make_response):
    url = env.get('PATH_INFO')
    view_func = url_map.get(url)
    if not view_func:
        make_response('404 not found', [('content-type', 'text/plain')])
        return [b'page not found']
    make_response('200 ok', [('content-type', 'text/plain')])
    resp = view_func().encode('utf-8')
    return [resp]
```

### 3.3、通过装饰器实现简易的路由注册

```
"""实现简单的路由系统"""

routers = {}

def add_url(url, view_func):
    """添加路由"""
    routers[url] = view_func

def route(rule, **option):
    """路由注册装饰器"""
    def wrapper(f):
        add_url(rule, f, **option)
        return f
    return wrapper

@route("/")
def index():
    return "hello world"

@route('/hello')
def hello():
    return "hi"

if __name__ == '__main__':
    while True:
        url = input("请输入要访问的 URL:")
        if url in routers:
            print(routers[url]())
        else:
            print("not found")
```



### 3.4、flask 设置路由

装饰器注册

- 源码
- add_url_rule

集中注册

- 核心语句：self.url_map.add(rule)
  - 在路由注册前后打印 app.url_map
  - 不同在于封装了一个 Map 类，也可以用列表字典现成的。
- self.view_functions[endpoint] = view_func
  - wsgi_app() 调用：
  - self.full_dispatch_request()
  - def dispatch_request(self): 调用了视图函数。

什么时候用集中注册，什么时候用装饰器：

- 大型项目
- 中型项目



问题

- 一个视图函数可以对应多个 URL 吗
- 一个 URL 可以对应多个视图吗？



URL映射

- app.url_map 保存url和 endpoint 关系
- app.view_functions 保存 endpoint 和 函数关系。



装饰器的装饰

还有其他的装饰器怎么处理？视图函数放最外层？

- 1、视图装饰器应该放最外层，否则里面的装饰器不会生效。
- 2、视图函数包裹的装饰器不要 return 值，否则会被包装成返回数据。

```
def log_time(f):
    def swapper(*args, **kw):
        print(f'{time.time()}')
        return f(*args, **kw)
        # （不要return 其他的，否则会被作为response 包装）
    return swapper


@app.route('/') # 换下位置
@log_time
def get_projects(p_id=7):
    return f'hello {p_id}'
```



### 3.5、类视图

类视图有什么好处

前后端分离，

- 类是可以继承的
- 代码可以复用
- 可以定义多种行为 类：方法，行为， 属性：特征
- 类：不同的请求方式：get, post, delete



可插拔视图

```
 from flask.views import View 
 app = Flask(__name__) 
 class UserView(View):   
    # 定义允许的方法 
     methods = ['GET', 'POST'] 
     def dispatch_request(self): 
         return '{} 到 hello'.format(request.method) 
 app.add_url_rule('/', view_func=UserView.as_view('user'))
```

看源码：

```
 methods  # 定义支持的请求方式。 
 dispatch_request  # 分配请求。 
 decorators = [superuser_required] # 装饰器
```

而 as_view 会把 user endpoint 和 view 方法绑定在一起，类方法的视图函数都是

view 方法。



对不同的请求方式进行不同的处理

```
def get(self): 
     return 'This is Get' 
  
 def post(self): 
     return 'This is Post' 
  
 def dispatch_request(self): 
     # 改进：如果 method.lower() 不是一个字符串，不需要判断， 
     # 执行 method() 就可以了。 
     method = request.method 
     if method == 'GET': 
         return self.get() 
     elif method == 'POST': 
         return self.post()
```

TODO: 能不能用 __call__ 分配



MethodView

```
 class UserView(MethodView): 
     # methods = ['GET', 'POST'] 
     # 不需要了，会自己去调用判断。 
  
     def get(self): 
         return 'This is Get' 
  
     def post(self): 
         return 'This is Post'
```



自己实现的 MethodView

```
class MethodView(View):

    methods = ['get', 'post', 'put', 'delete']

    def dispatch_request(self):
        method = request.method.lower()
        view_func = getattr(self, method, '')
        if (method in self.methods) and view_func:
            return view_func()
        else:
            return {"msg": 'method not allowd'}

class ProjectView(MethodView):
    def get(self):
        return 'get'
```



装饰视图类

- 不能直接在类上添加装饰器
- 不能直接在方法上添加装饰器
- 因为装饰的是视图函数。 UserView.as_view("user")

1、第一种：手工, 装饰的时候类视图一定要先允许 post 请求，比如使用 methods=, 或者 method view

```
 def post_only(f): 
     def wrapper(*args, **kw): 
         if request.method == 'POST': 
             return f(*args, **kw) 
         abort(405) 
     return wrapper 
  
  
 f = UserView.as_view('user') 
 f = post_only(f)   
 app.add_url_rule('/', view_func=f)
```

**已提交issue,是自己调用顺序的问题**

```
def post_required(f):
    def decorator(*args, **kw):
        print(request.method)
        return f(*args, **kw)
    return decorator


class ProjectView(MethodView):

    def get(self):
        return "get method"

    def post(self):
        return "post method"

p_view = post_required(ProjectView.as_view("project"))
app.add_url_rule('/project', view_func=p_view)
```

2、第二种：

```
&nbsp;decorators  = [get_only]
```



MethodView 的例子 REST

设计 测试项目里面的 API:

[Untitled](https://ky8tjff7tg.feishu.cn/docs/Flask 0600d0056f1142e0b5d56fed148c0090/Untitled Database 0f5e048e3e594bd18196aa1f03266da4.csv)



传统的视图函数（通常用在前后端不分离的情况）

1、通过 斜杠 传递参数：

```
 @app.route('/projects/<project_id>', methods=['GET', 'POST', 'PUT', 'DELETE']) 
 def project(project_id): 
     method = request.method 
     if method == 'GET': 
         return f'get {project_id}' 
     elif method == 'POST': 
         return f'POST {project_id}' 
     return 'hello' 
  
  
 @app.route('/projects/', methods=['GET',]) 
 def projects(): 
    return 'get all projects'
```

2、通过 ? 号 和 表单传递传递参数：

```
 @app.route('/projects/', methods=['GET', 'POST', 'PUT', 'DELETE']) 
 def project(): 
     method = request.method 
     project_id = request.form.get('p_id') 
     if method in ['POST', 'PUT', 'DELETE'] and project_id is None: 
         return 'not allowed' 
     if method == 'GET': 
         project_id = request.args.get('p_id') 
         if project_id is None: 
             return 'get all projects' 
         return f'get {project_id}' 
     elif method == 'POST': 
         return f'POST {project_id}' 
     return 'hello'
```

3、通常是获取所有，获取一个，创建都单独创建一个。

### MethodView

斜杠形式：

```
 class UserView(MethodView): 
  
     def get(self, project_id=None): 
         if project_id is None: 
             return 'Get all project' 
         return f'get {project_id}' 
   
     def post(self): 
         return f'post ' 
  
     def put(self, project_id): 
         return f'put {project_id}' 
  
     def delete(self, project_id): 
         return f'delete {project_id}' 
  
 f = UserView.as_view('project') 
 app.add_url_rule('/projects/<project_id>', view_func=f, methods=['GET', 'POST', 'PUT', 'DELETE']) 
 app.add_url_rule('/projects/', defaults={"project_id":None}, view_func=f, methods=['GET', ])
```

问号形式：

```
 class UserView(MethodView): 
  
     def get(self): 
         project_id = request.args.get('p_id') 
         if project_id is None: 
             return 'Get all project' 
         return f'get {project_id}' 
  
     def post(self): 
         return f'post ' 
  
     def put(self): 
         project_id = request.form.get('p_id') 
         return f'put {project_id}' 
  
     def delete(self): 
         project_id = request.form.get('p_id') 
         return f'delete {project_id}' 
  
 f = UserView.as_view('user') 
 app.add_url_rule('/projects/', view_func=f, methods=['GET', 'POST', 'PUT', 'DELETE'])
```





## 4、蓝图

### 视图分离

views 和 app 会造成循环导入

### 

项目结构

- app.py / wsgi.py 服务器
- README.md 
- .gitignore
- requirements.txt
- LemonTest/
  - __init__.py
  - extensions.py
  - config.py 
  - blueprints/



### 蓝图的基本结构

- /api/v0/
- /api/v1/
- /web/
- /admin/

### 蓝图初始化

```
# blueprint.py
web_bp = Blueprint('web', __name__, url_prefix='/web', template_folder='templates')
```

- template_folder 可以用默认的 app 路径，不填
- 也可以为蓝图单独设置，路径是相对该蓝图的根路径的相对路径。

### 蓝图注册

```
app.register_blueprint(web_views.web_bp)
```

### 循环导入

- 蓝图的初始化需要引入 views, views里面又需要导入蓝图

### 避免循环导入的策略

- 加文件



### 蓝图当中的文件结构

- views.py
- blueprint.py
- models.py
- …

### 使用蓝图创建登录注册接口

通过 [接口文档](http://www.keyou.site:8000/) 查看：

```
@user_bp.route('/register/', methods=['OPTIONS', 'POST'])
def register():
    return {"access_token": ""}


@user_bp.route('/login/')
def login():
    return {"access_token": ""}


@user_bp.route('/&lt;username&gt;/count/')
def validate_username(username):
    return {
        "username": username,
        "count": 0
    }


@user_bp.route('/&lt;email&gt;/count/')
def validate_email(email):
    return {
            "email": email,
            "count": 0
        }
```



## flask-cors 的使用

在 extensions.py 中初始化

```
cors = CORS()

# app的初始化文件
cors.init_app(app)
```





## 5、Flask 配置文件

### PY 配置文件

- 配置文件项大写
- 是常量

```
# setting.py
DEBUG = True

# run.py
from setting import DEBUG
app.run(debug=DEBUG)
```

### YAML 配置文件

```
with open('app/conf.yaml') as f:
    config_data = yaml.load(f, Loader=yaml.SafeLoader)

app.config.from_mapping(config_data)
```

### 导入方式

- app.config.from_ 通过过 app.config 获取配置
- 基本不用 python 模块导入方式获取配置
  - 每个模块都要导入，太麻烦

### 其他方式

- from_envvar
- from_pyfile 以上都是用 from_object
- from_mapping
- from_json 也是用 from mapping
- fromkeys

### 配置项注意事项

- 必须大写
- 因为是常量

再 Flask 当中，在配置文件 setting 当中 使用 Debug = True，在核心对象当中执行

```
app.config.from_object('setting')
print(app.config['Debug'])
print(app.config['DEBUG'])
```

会打印什么？

使用以下方式会打印什么？

```
import setting
print(settting.Debug)
print(setting.DEBUG)
```

### 默认配置

**SECRET_KEY**

**JSON_AS_ASCII** 默认是 True， 改成 false 是 utf-8, 可以显示中文。

```
app.config.update(JSON_AS_ASCII=False)

@app.route("/")
def index():
    user = {"name":"雨泽","age":1}
    return jsonify(user)
```

### 配置的最佳实践

配置应该是灵活的，最少应该考虑到生产环境和开发环境的不同，如果再加上测试环境会更好。

如果切换了不同的环境，不应该去频繁修改代码导入不同的配置项，而应该是提前规划好。

在一个 [config.py](http://config.py/) 文件中分别配置好通用的选项，生产环境选项和开发环境选项。开发环境和生产环境可以继承通用配置。

```
class Config:
    SECRET_KEY = os.urandom(32)
    DEBUG = False
    JWT_HEADER_TYPE = 'JWT'


class DevConfig(Config):
    DEBUG = True
    SQLALCHEMY_TRACK_MODIFICATIONS = True
    SQLALCHEMY_DATABASE_URI = r"sqlite:///LemonPlatform.db"
    SECRET_KEY = "your secret key"

class ProConfig(Config):
    pass
```

在创建 app 对象的工厂函数中提供 config 参数，提供默认值。比如默认提供生成环境，如果 config 参数传入了其他的配置，则使用其他环境。

```
from flask import Flask
from app import config as conf_module

app = Flask(__name__)

def create_app(config=None):
    if config:
        app.config.from_object(conf_module.ProductConfig)
    return app
```

之后，在不同的环境中调用 create_app 函数， 在生成环境中，运行 [wsgi.py](http://wsgi.py/) 文件。

```
# wsgi.py 
from app import create_app
app = create_app()
app.run()
```

在生产环境中会使用 gunicorn ，uwsgi 这样的服务，不会使用 flask 自带的服务器。这里只是演示。

在开发环境中，运行 [app.py](http://app.py/) 文件。

```
# app.py
from app import create_app
from app import config

app = create_app(config.DevelopConfig)

app.run()
```



## 使用 flask-sqlalchemy

### 配置文件设置

下面是获取现有的数据库操作：

```
# mysql以数据库名称+引擎的方式
app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql+pymysql://root:123@localhost:3306/demo'
SQLALCHEMY_TRACK_MODIFICATIONS = True

# sqlite
app.config['SQLALCHEMY_DATABASE_URI'] = r"sqlite:///C:\Users\muji\Desktop\SqlNotes.db"
```

### 插件初始化

```
# extensions.py
from flask_sqlalchemy import SQLAlchemy
db = SQLalchemy()
```

### app 注册插件

```
def init_extensions():
    db.init_app(app)
```

### models 定义

```
from application.extensions import db

class Product(db.Model):
    product_id = db.Column(db.Integer, primary_key=True)
    product_name = db.Column(db.String(80))
```

模型层创建以后一定要在 views 中导入使用，否则 migrate 不会生效。

### 使用模型层

```
@app.route('/')
def index():
    products = Product.query.all()
    return {"name": [p.product_name for p in products]}
```

### base 模型

```
class Base(db.Model):

    __abstract__ = True

    id = db.Column(db.Integer, primary_key=True)
    status = db.Column(db.SmallInteger, nullable=False, default=0)
    created_at = db.Column(db.DateTime, default=datetime.now())
    updated_at = db.Column(db.DateTime, default=datetime.now(), onupdate=datetime.now())
```

## 创建数据库

### 手工创建（一般不用）：

```
# 方法一
set FLASK_APP=run.py

flask shell 进入 python shell
from run import db
db.create_all()

# 方法二
with app.app_context as ctx:
        db.create_all()
```

### flask-migrate

```
# extensions.py
migrate = Migrate()

# init
migrate.init_app(app, db)

# flask db --help
```

常用命令

- init 创建文件夹和初始化工作
- migrate 根据模型和配置生成脚本
- upgrade 执行脚本，生成数据库
- downgrade 回滚。

\> 注意：主程序包名不能叫 app, 你可以换成任意其他的名字，因为会和 app 对象冲突。

#### 1、最常用的数据格式

| **Integer**   | **一个整数**                             |
| ------------- | ---------------------------------------- |
| String (size) | 有长度限制的字符串                       |
| Text          | 一些较长的 unicode 文本                  |
| DateTime      | 表示为 Python datetime 对象的 时间和日期 |
| Float         | 存储浮点值                               |
| Boolean       | 存储布尔值                               |
| PickleType    | 存储为一个持久化的 Python 对象           |
| LargeBinary   | 存储一个任意大的二进制数据               |

#### 2、参数

- db.ForeignKey('project.id')
- primary_key，主键，唯一标致
- autoincrement， 自增长
- unique，唯一
- index，索引
- nullable，可以为空
- default，默认值
- comment 说明，注释

#### 3、 查询和过滤

*提示*： 可以通过 [官方文档](https://docs.sqlalchemy.org/en/13/orm/query.html?highlight=filter#sqlalchemy.orm.query.Query.filter) 或者 Query 源码查看相关函数

ORM, 

1、所有的 all()

```
users = User.query.all()
# Session
select * from user
```

2、第一个 first()

```
users = User.query.first()
```

3、get() 通过主键去获取

```
users = User.query.get(1)

# 如果有多个主键
users = User.query.get((1,5))
users = User.query.get({"id":1,"project_id":3})
```

4、filter_by()

```
admin = User.query.filter_by(username='admin').first()
```

5、filter() 更加复杂的查询

[返回真假](https://docs.sqlalchemy.org/en/13/orm/query.html)

```
User.query.filter(User.email.like('@example.com')).all()
```

*提示*： 支持的字段操作：ColumnOperators 源码。

6、按某种规则对用户排序:

```
&gt;&gt;&gt; User.query.order_by(User.username.desc()).all()
[&lt;User u'admin'&gt;, &lt;User u'guest'&gt;, &lt;User u'peter'&gt;]
```

7、限制返回用户的数量: 应用场景：20，offset()

```
&gt;&gt;&gt; User.query.order_by(User.username.desc()).limit(1).all()
[&lt;User u'admin'&gt;]
```

*链式调用*

#### 8, 使用上下文管理器提交事务

```
try:
    db.session.add(user)
    db.session.commit()
except:
    db.session.rollback()
```

每次都要用 try … accept…很烦，实际上每次都是重复代码，对前后都有一些固定的代码需要执行的，可以使用上下问管理器

```
@contextmanager
def auto_commit():
    try:
        yield
        db.session.commit()
    except:
        db.rollback()

# 使用
with auto_commit():
    db.session.add(user)

# yield 讲解
```

### 七、原生SQL 语句

多表查询，sql, ORM, execute() DBAPI

db.session.execute()

```
def select():
    with app.app_context() as ctx:
        sql = 'select * from user;'
        a = db.session.execute(sql)
        #  a 是一个 ResultProxy
        # print(a.fetchall())
        # print(a.fetchone())
```

用原生 sqlalchemy 执行sql 语句：

db.session.execute()

```
def init_sql():
    import sqlalchemy

    db_engine = sqlalchemy.create_engine('mysql+pymysql://root:@localhost/weibo', echo=True)
    db_conn = db_engine.connect()
    a = db_conn.execute('select * from user;')
    print(a.fetchall())
```

原生 sql 语句的参数化：

```
session = Session(bind=engine)
sql = 'select * from user where name = :name;'
a = session.execute(sql, params={'name':'yuz'})
print(a.fetchall())
```

## 分页 (TODO)

在试图中使用：

```
pag = Projects.query.paginate(1,1,error_out=False)
projects = pag.items
```

封装：

```
 @classmethod
    def paginate(cls, page=1, **filter_by):
        per_page = current_app.config.get('PER_PAGE', 2)
        return cls.query.filter_by(**filter_by).order_by(desc(cls.updated_at)).paginate(
            int(page), per_page=per_page, error_out=False
        )
```

视图函数中：

```
page = User.paginate(1)
users = page.items
print(users)
```

## 实现 projects 的创建，修改

### model

```
class Project(Base):
    name = db.Column(db.String(200), unique=True)
    leader = db.Column(db.String(50))
    tester = db.Column(db.String(50))
    programmer = db.Column(db.String(50))
    publish_app = db.Column(db.String(100))
    desc = db.Column(db.String(200))

    def to_json(self):
        return {
            "id": self.id,
            "name": self.name,
            "leader": self.leader,
            "tester": self.tester,
            "programmer": self.programmer,
            "publish_app": self.publish_app,
            "desc": self.desc
        }
```

### 视图函数

```
@api_v0.route('/projects/')
def project_list():
    projects = Project.query.all()
    return {"results": [p.to_json() for p in projects]}


@api_v0.route('/projects/', methods=['POST'])
def project_create():
    req_data = request.json
    project = Project(**req_data)
    project.save()
    return {"results": project.id}


@api_v0.route('/projects/names/', methods=['OPTIONS', 'GET'])
def projects_names():
    projects = Project.query.all()
    return jsonify([p.to_json() for p in projects])


@api_v0.route('/projects/&lt;id&gt;/', methods=['GET'])
def projects_get(id):
    project = Project.query.get(id)
    return {"results": project.to_json()}
```

### 保存数据函数

```
def save(self):
    try:
        db.session.add(self)
        db.session.commit()
        return self
    except:
        print("正在回退。")
        db.session.rollback()
        raise ValueError()
```

## 模型关系

```
class Envs(Base):
    name = db.Column(db.String(200), unique=True)
    base_url = db.Column(db.String(200), comment='请求base url')
    desc = db.Column(db.String(200), comment='简要描述')


class Projects(Base):
    name = db.Column(db.String(200), unique=True)
    leader = db.Column(db.String(50))
    tester = db.Column(db.String(50))
    programmer = db.Column(db.String(50))
    publish_app = db.Column(db.String(100))
    desc = db.Column(db.String(200))
    interfaces = db.relationship("Interfaces", backref='project')


class Interfaces(Base):
    name = db.Column(db.String(200), unique=True, comment='接口名称')
    tester = db.Column(db.String(50), comment='测试员')
    desc = db.Column(db.String(200), comment='接口描述')
    project_id = db.Column(db.ForeignKey('projects.id'))
    testcases = db.relationship("Testcases", backref='interface')
    configs = db.relationship("Configures", backref='interface')


class Testcases(Base):
    name = db.Column(db.String(200), unique=True, comment='用例名称')
    author = db.Column(db.String(50), comment='编写人员')
    request = db.Column(db.String(200), comment='请求信息')
    include = db.Column(db.String(200), comment='用例执行前置顺序')
    interface_id = db.Column(db.ForeignKey('interfaces.id'))

class Reports(Base):
    name = db.Column(db.String(200), unique=True, comment='报告名称')
    result = db.Column(db.SmallInteger, default=1, comment='执行结果')   # 1为成功, 0为失败
    count = db.Column(db.Integer, comment='总用例数')
    success = db.Column(db.Integer, comment='成功总数')
    html = db.Column(db.Text, comment='报告HTML源码', default='')
    summary = db.Column(db.Text, comment='报告详情', default='')


class DebugTalks(Base):
    name = db.Column(db.String(200), default='debugtalk.py', comment='debugtalk文件名称')
    debugtalk = db.Column(db.Text, default='#debugtalk.py', comment='debugtalk.py文件')
    project_id = db.Column(db.ForeignKey("projects.id"))


class Configures(Base):
    name = db.Column(db.String(50),  comment='配置名称')
    author = db.Column(db.String(50), comment='编写人员')
    request = db.Column(db.Text)
    interface_id = db.Column(db.ForeignKey('interfaces.id'))
```



## 注册流程

### User 类

```
from werkzeug import security
from LemonTest.models import Base, db


class User(Base):
    username = db.Column(db.String(200), unique=True)
    password = db.Column(db.String(512))

    @property
    def pwd(self):
        raise AttributeError("不能获取密码")

    @pwd.setter
    def pwd(self, data):
        self.password = security.generate_password_hash(data)

    def check_password(self, data):
        return security.check_password_hash(self.password, data)
```

### 视图函数

注册：

```
@api_v0.route('/user/register/', methods=['POST'])
def register():
    req_data = request.json
    user = User(
        username=req_data.get('username')
    )
    # 修改 pwd
    user.pwd = req_data.get('password')
    user.save()
    return {"access_token": ""}
```

登录：

```
@api_v0.route('/login/', methods=['POST'])
def login():
    req_data = request.json
    user = User.query.filter_by(username=user_obj['username']).first()
    if user.check_password(req_data["password"]):
        token = create_access_token(identity=user.username)
        return {"token": token, "username": user.username}
    return {"msg": "error"}
```

## 使用 md5/sha256 自己加密可以吗？

```
salt = 'abc'

def generate_password_hash(password):
    """密码"""
    hash = hashlib.sha256()
    hash.update((password + salt).encode('utf-8'))
    return hash.hexdigest()

def check_password(hashpwd, password):
    hash = generate_password_hash(password)
    return hash == hashpwd
```

## jwt 安装

```
pip install flask-jwt-extended
```

## 快速使用



- JWT_SECRET_KEY 用于加密
- JWT_ACCESS_TOKEN_EXPIRES 设置加密过期时间。
- login 接口通过 create_access_token 生成 token
- jwt_requited 标明需要 token 校验
- 接口当中通过 get_jwt_identity 解密 token

```
app.config['JWT_SECRET_KEY'] = 'fow'


@app.route('/login', methods=['POST'])
def login():
    token = create_access_token(identity='yuz')
    return {"token": token}

@app.route('/')
@jwt_required
def index():
    user = get_jwt_identity()
    return "hello world"
```

## 修改 header 类型

默认是使用 Bearer 类型

```
JWT_HEADER_TYPE = 'JWT'
```

## 修改 callback

token 破解后默认通过 get_jwt_identity 得到原始身份信息，

如果想去获取是哪个用户只能写代码通过数据库查找，而通过装饰器，可以触发回调。

你可以在视图当中通过 jwt 模块中的 get_current_user 方法，或者 current_user 变量获取到用户。

```
def register_extensions():
    """添加插件"""
    cors.init_app(app)

    @jwt.user_loader_callback_loader
    def user_loader_callback(identity):
        user = User.query.filter_by(username=identity).first()
        if user:
            return user
```



# 使用 marshmallow 序列化和数据校验

marshmallow 是一个非常优秀的 python 序列化工具，也能欧进行数据校验。 marshmallow 吸收了 django rest framework, flask restful 等优秀框架的思想，api 简单易用，

不和任何平台或框架绑定。 只要是在 python 当中进行序列化或者数据校验，都可以考虑 marshmallow。

## 序列化（Serialization）

序列化是指把编程语言的对象转化成通用的数据，比如把对象转化成二进制流，或者在web 应用中把对象转化成 json 等通用格式,在 marshmallow 中用 dump 进行序列化。

使用 marshmallow 进行序列化：

- 1，编写 Schema ,用于数据转化
- 2，准备需要序列化的对象
- 3，使用 marshmallow.dump 进行序列化

注意在序列化阶段，默认你的对象数据合法，直接根据字段转化成通用的数据格式，不会对数据合法性校验了。

![img](https://ky8tjff7tg.feishu.cn/space/api/box/stream/download/asynccode/?code=NWYzNzE2MzNmODQ4YzU4YWIzOTM1M2NmMWYzZjYxMjhfTVQ1b1dESnFwQXpYTExHemtzdFhDdGxpVG41MmVkVFJfVG9rZW46Ym94Y240VzVWcnNBWU4zQlJLNUk4WU9DMkllXzE2MTcxMDYyNzI6MTYxNzEwOTg3Ml9WNA)

```
from marshmallow import schema, fields


class User:
    def __init__(self, username, password, age):
        self.username = username
        self.password = password
        self.age = age

    def __repr__(self):
        return f"User&lt;username={self.username}&gt;"


class UserSchema(schema.Schema):
    username = fields.Str()
    password = fields.Str()
    age = fields.Int()


schema = UserSchema()
user1 = User(username='yuz', password='123', age=14)

# 序列化成功
user1_serial = schema.dump(user1)
print(user1_serial)
```

**说明**：

- UserSchema 中的字段名称和 User 的属性名称一致
- fields.Str() 表示字段必须能够转化成字符串。fields.Int() 必须能转化成整型
- 当因为 fields 类型不满足序列化失败的时候，会抛出 ValueError 异常

```
# 序列化失败
user2 = User(username='yuz', password='123', age='not a int')
user2_serial = schema.dump(user2)
print(user2_serial) # 报错
```

## 序列化失败的异常处理

当序列化失败时，会抛出异常。比如 filed 转化失败会抛出 ValueError 异常。

可以通过捕获异常获取错误原因, err.args 得到错误原因

TODO:(不明白为什么 marshmallow 序列化失败要抛出 ValueError , 而不是统一用 ValidationError)

```
# 序列化失败
user2 = User(username='yuz', password='123', age='not a int')
try:
    user2_serial = schema.dump(user2)
except Exception as err:
    print(err.args)
```

## 序列化的数据过滤（Filtering）

序列化必须要考虑安全和带宽问题。敏感数据不能暴露的应该过滤，或者混淆加密。还有一些冗余数据没有必要暴露的有要过滤。

比如 User 当中的 password 应该被过滤。

![img](https://ky8tjff7tg.feishu.cn/space/api/box/stream/download/asynccode/?code=ODQ1ZjQ5NDM2YzE5MmRiNWJjOTExMjAyYzgwY2Y5M2JfaUxOY3ZNcmhYMk9WZDVWUHpTU1NJT1VocmtEeGNLUnNfVG9rZW46Ym94Y242WjZUNGJ1MDY4TG0zWWpPVk9GeExmXzE2MTcxMDYyNzI6MTYxNzEwOTg3Ml9WNA)

数据过滤有 2 中形式：

- 通过 only 关键参数选择需要序列化的字段白名单，数据类型是元组或者列表
- 通过 exclude 关键字参数选择需要过滤掉的字段黑名单，数据类型是元组或者列表

```
# 通过 only 关键字参数选择需要序列化的字段
schema = UserSchema(only=('username', 'age'))
user_serial_filter = schema.dump(user1)
print(user_serial_filter)
# 通过 exclude 过滤
schema = UserSchema(exclude=('password',))
user_serial_filter = schema.dump(user1)
print(user_serial_filter)
```

## 序列化多个数据

现在有多个 User 对象需要同时序列化，这在 web 应用当中获取列表信息时总会用到。

多数据序列化可以在 Schema 初始化时传入 many=True 参数，执行 dump 时传入列表就可以了。

```
schema = UserSchema(many=True)
user1 = User(username='yuz', password='123', age=14)
user2 = User(username='yuz', password='123', age=15)
users_serial = schema.dump([user1, user2])
print(users_serial)
```

user1 和 user2 都序列化成功，如果其中有一个没有通过，则会引发异常。

那有没有一种方法忽略引发异常的单个数据，返回能正常序列化的数据呢？

TODO:(通过阅读源码发现每个字段不符合就会报错，没有对错误进行统一的调度。先在 stackoverflow 和 github 提问)

issue: https://github.com/marshmallow-code/marshmallow/issues/1647

提交 question, 是自己对序列化的理解有误。序列化认为数据已经是合法的对象，不应该在做数据校验，只是根据对应的字段转化成通用数据类型。

```
schema = UserSchema(many=True)
user1 = User(username='yuz', password='123', age=14)
user2 = User(username='yuz', password='123', age="not a int")
users_serial = schema.dump([user1, user2])
print(users_serial)
```

## 反序列化（Deserialization）

反序列化是指把通用数据转化成编程语言的对象，比如把二进制流转化成对象，或者在web 应用中把 json 等通用格式转成对象。

![img](https://ky8tjff7tg.feishu.cn/space/api/box/stream/download/asynccode/?code=MzRlZDUzZGNjYTM3ZmI5ZWFiNGQzMmNkN2U0NmE3ZTNfNUw3cWR6d0JUNHBPdDBxbE1mVE00VWpmMTJ6ckN3WWpfVG9rZW46Ym94Y240U1F2MVhmSVZVcTNsT0ZSZTZOSEVkXzE2MTcxMDYyNzI6MTYxNzEwOTg3Ml9WNA)

在 marshmallow 中，load 方法默认会对字典进行校验，转化后还是字典,只起到了数据校验的作用。

如果是要对 json 字符串进行校验，可以使用 loads 方法。

```
# dict to dict
user_from_dict = {"username": "yuz", "password": "123"}
schema = UserSchema()
user = schema.load(user_from_dict)
print(user)

# json string to dict
json_user = '{"username": "yuz", "password": "123"}'
schema = UserSchema()
user = schema.loads(json_user)
print(user)     
```

## 反序列化中没有 Schema 字段

传入的原始数据没有 age 字段，是没有问题的，但是如果有多余的字段而 schema 中没有定义，则不可以。

此时会报 ValidationError 错误

```
user_from_dict = {"username": "yuz", "password": "123", "gender": "男"}
schema = UserSchema()
user = schema.load(user_from_dict)
print(user)
```

## 反序列化失败

反序列化失败会报 ValidationError错误， 和序列化的 ValueError 不一样。(TODO: Why)

## 反序列化成 ORM 对象

基本上不会去干把字典转化成字典的事，反而是在 web 应用中需要校验用户传输的数据是不是能够转化成数据库的 ORM 模型对象。假设 User 类就是 ORM.

要转化成 ORM 需要做：

- 通过 post_load 声明在调用 load 时的行为。（也可以通过 post_dump 装饰器进行序列化操作）
- 在 Schema 中定义方法，决定转化成什么对象。
- 行为当中传入 value 参数和 **kwargs, 注意 **kwargs 不能省略

**使用 flask-marshmallow 等集成库使用会更简单**

```
from marshmallow import post_load, schema


class UserSchema(schema.Schema):
    username = fields.Str()
    password = fields.Str()
    age = fields.Int()

    @post_load
    def load_to_orm(self, value, **kwargs):
        user = User(**value)
        user.save()
        return user


user_from_dict = {"username": "yuz", "password": "123", "age":14}
schema = UserSchema()
user = schema.load(user_from_dict)
print(user)
```

## ORM 中异常处理

```
from marshmallow import post_load, schema, ValidationError, fields


class UserSchema(schema.Schema):
    username = fields.Str()
    password = fields.Str()
    age = fields.Int()

    @post_load
    def load_to_orm(self, value, **kwargs):
        try:
            return User(**value)
        except Exception as e:
#             raise ValidationError(str(e))
            print(f"不能完成对象转化{e}")


user_from_dict = {"username": "yuz", "password": "123"}
schema = UserSchema()
user = schema.load(user_from_dict)
print(user)
```

\> 注意：load_to_orm(self, value, **kwargs): 一定要带 **kwargs 

## 反序列化的多值处理

多值处理如果有一个数据不正确，则会抛出异常，

如果你想忽略有错误的数据，可以进入 err , err.messages 显示 {1:{"age", "not a int"}}, 会把有问题的数据索引列举出来，根据索引删除对应的数据即可。

```
user1 = {"username": "yuz", "password": "123", "age": 12}
user2 = {"username": "demo", "password": "456",  "age": "not a int"}
schema = UserSchema(many=True)
user = schema.load([user1, user2])
print(user)
```

## 内置的常用 Field

- Str
- Int
- Date
- URL
- Email

## Field 常用参数

- required
- validate

## required 参数

表示在进行反序列化中，必须要有相关字段，否则无法转化成对应的对象， 比如在 ORM 转化 load_to_orm 中，由于 age 没有传入会导致 User 对象初始化失败，

所以应该把 schema 当中的字段都设置成 required。当没有传入相关字段时，在初始化 User 之前就会包 ValidatrionError 错误。

```
from marshmallow import post_load, schema, ValidationError


class UserSchema(schema.Schema):
    username = fields.Str(required=True)
    password = fields.Str(required=True)
    age = fields.Int(required=True)

    @post_load
    def load_to_orm(self, value, **kwargs):
        return User(**value)


user_from_dict = {"username": "yuz", "password": "123"}
schema = UserSchema()
user = schema.load(user_from_dict)
print(user)  # 报 ValidationError 错误
```

**required 参数在序列化时无效**

```
class User:
    def __init__(self, username):
        self.username = username

user = User("yuz")
schema = UserSchema()
serial_data = schema.dump(user)
print(serial_data)  # 
```

## 内置校验器 Validator

如果对数据字段需要进一步校验，需要用到 field 当中的 validate 参数。

marshmallow 的数据校验写法充分吸收了 django rest framework 和 flask wtform 的写法长处。

注意：数据校验都是针对反序列化 load 操作， 序列化操作是没有数据校验的。

常用的校验器有:

- Length
- Range
- Equal
- OneOf
- URL
- Email
- Regexp

```
from marshmallow import schema, fields, validate


class UserSchema(schema.Schema):
    username = fields.Str(validate=[validate.Length(max=64)])
    password = fields.Str(validate=[validate.Regexp(r"/^(?=.*[a-zA-Z])[\s\S]{8,32}$/")])
    age = fields.Int(validate=[validate.Range(min=6,max=200)])
    gender = fields.Str(validate=[validate.OneOf(['男', '女', '未知'])])


schema = UserSchema()
user = {"username": "hey", "password": "123", "age": 5, "gender": "invalid option"}

# 序列化会正常，
serial = schema.dump(user)
print(serial)  

# 反序列化失败
obj = schema.load(user)
print(obj)
```

## 自定义校验器 validator

有时候你需要定制某个字段需要符合的规则，可以自己定义一个函数做为 validator, 接收 value 为参数，当不符合规则是，抛出异常。

```
from marshmallow import schema, fields, validate, ValidationError


def in_list(value):
    allowed_data = ['yuz', 'demo']
    if value not in allowed_data:
        raise ValidationError('不是指定的用户')


class UserSchema(schema.Schema):
    username = fields.Str(validate=[in_list, validate.Length(max=2)])
    password = fields.Str(validate=[validate.Regexp(r"/^(?=.*[a-zA-Z])[\s\S]{8,32}$/")])
    age = fields.Int(validate=[validate.Range(min=6,max=200)])
    gender = fields.Str(validate=[validate.OneOf(['男', '女', '未知'])])


schema = UserSchema()
user = {"username": "hey"}


# 反序列化失败
obj = schema.load(user)
print(obj)
```

## 使用装饰器自定义 validator

还有另外的一种方式提供自定义的校验器。

可以在 Schema 下定义一个方法，用 validators 装饰器装饰。

但是这种方式不能为同一个字符设置多个方法校验，只有一个生效。不灵活。

```
from marshmallow import schema, fields, validate, ValidationError, validates


class UserSchema(schema.Schema):
    username = fields.Str()
    password = fields.Str(validate=[validate.Regexp(r"/^(?=.*[a-zA-Z])[\s\S]{8,32}$/")])
    age = fields.Int(validate=[validate.Range(min=6,max=200)])
    gender = fields.Str(validate=[validate.OneOf(['男', '女', '未知'])])

    # validates 参数为需要校验的字段
    @validates('username')
    def validate_username_in_list(self, value):
        allowed_data = ['yuz', 'demo']
        if value not in allowed_data:
            raise ValidationError('又不是指定用户')

    @validates('username')
    def validate_username_length_limit(self,value):
        if len(value) &lt; 2:
            raise ValidationError('数据少于 2 个字符')


schema = UserSchema()
user = {"username": "hey"}


# 反序列化失败
obj = schema.load(user)
print(obj)
```

## flask-marshmallow 的使用

该插件主要对 flask 框架做了一些集成，当使用 orm 的时候可以根据 orm 对象自动生成 Schema 校验字段。

在反序列化时，不会自动转成 orm 对象，可以自己通过 @post_load 装饰器实现。

\> 注意：初始化的时候一定要先安装 marshmallow-sqlalchemy， 这个库，否则会报错误。

\>

\> Marshmallow 对象没有 SQLAlchemyAutoSchema 属性

```
ma = Marshmallow(app)


class AuthorSchema(ma.SQLAlchemySchema):
    class Meta:
        model = Author

    id = ma.auto_field()
    name = ma.auto_field()
    books = ma.auto_field()


class BookSchema(ma.SQLAlchemyAutoSchema):
    class Meta:
        model = Book
        include_fk = True
        fields = ()

    @post_load
    def to_orm(self, value, **kw):
        pass
```

- 通常来说，不需要通过 fileds = ("username",) 过滤，而是在初始化的时候提供 only 或者 exclude 参数。

```
schema = BookSchema(only=())
```

- 在反序列化 load 时，需要对数据校验，必须传入的参数可以通过 nullable 参数改变 ORM 对象， 数据校验时是通过 ORM 对象的参数要求校验的。

## Issues

- [#1650](https://github.com/marshmallow-code/marshmallow/issues/1650)
- [#1647](https://github.com/marshmallow-code/marshmallow/issues/1647)



## 为什么要有异常处理

```
from flask import Flask

app = Flask(__name__)

@app.route('/')
def index():
    1 / 0
    return "hello"

app.run(debug=True)
```

从结果上来看，这没有什么问题。但是从用户交互的角度来说，这样的报错信息是不友好的。

- 如果是 HTML 页面，你最好能返回一个更加精美的页面
- 如果是 API, 你最好能返回一个类似 json 这样的标准格式
- 你最好能给用户一点提示，是什么原因报的错，而不是让他们觉得你的服务真烂。

## Flask 的全局错误处理机制

Flask 提供了一种全局的错误处理机制，你可以通过 @app.error_handler 自定义不同的异常处理方式。

```
@app.errorhandler(ZeroDivisionError)
def zero_error_handler(e):
    return "您的数据运算不符合预期，请联系管理员...", 500
```

**说明**：

- @app.errorhandler() 参数是异常类或者是状态码（404）等
- 处理函数参数 e 表示接受的错误对象
- 返回值将会显示在前端的 Response 对象

你可以通过 errorhandler 自定义一个你觉得更好的页面：

```
@app.errorhandler(404)
def error_handler_404(e):
    # return render_template('404.html')
    return "&lt;html&gt;这个页面飞走了,请检查地址是否发生变化...&lt;/html&gt;"
```

如果是一个 API，你可以返回 json :

```
@app.errorhandler(404)
def error_handler_404(e):
    return {"msg": "not this page"}
```

统一处理:

```
@app.errorhandler(Exception)
def error_handler_all(e):
    return {"msg": "error found"}
```

## 多种错误类型的组合使用

在上面的例子中，我们对 ZeroDivisionError 进行了异常处理，但是如果有一些异常我提前没有设想到，

没有进行全局设置，那他还是会报不友好的信息。

现在，我们添加一个捕获所有异常的处理函数

```
@app.errorhandler(Exception)
def error_handler_all(e):
    return "服务正在维护...", 500

@app.errorhandler(ZeroDivisionError)
def zero_error_handler(e):
    return "您的数据运算不符合预期，请联系管理员...", 500
```

注意 ZeroDivisionError 是包含在 Exception 当中，也就是说当运算 1 / 0 的时候，

ZeroDivisionError 和 Exception 这 2 个异常都是满足的，那到底是触发哪个呢？

在 Flask 当中，当他能够直接在全局 errorhandler 中找到具体的错误类型 ZeroDivisionError，

就会触发这个，不会再去触发通用的 Exception.

而 Exception 的作用是去处理那些你实现没有想到的异常。

## json 错误信息返回

Flask 内置的错误处理都是使用 HTTPException 实现的，这个类返回的都是 HTML, 比如我们之前看到默认的 404 页面和 500 页面，

都是因为 Flask 内部定义了 NotFound 这样的子类。

如果项目都是采用 RESTful 风格的接口，不会返回 HTML 页面，那么这写内置的错误处理类都不需要了。

但是我们需要一个通用的返回 json 标准格式的异常类。

这个类可以直接继承 HTTPException, 只需要修改响应数据的对应方法。

```
from werkzeug.exceptions import HTTPException


class ApiHTTPError(HTTPException):

    def __init__(self, code=None, description=None, response=None):
        super().__init__(description, response)
        self.code = code

    def get_description(self, environ=None):
        return self.description

    def get_headers(self, environ=None):
        return [("Content-Type", "application/json; charset=utf-8")]

    def get_body(self, environ=None):
        return json.dumps({
            "code": self.code,
            "name": self.name,
            "description": self.get_description(environ),
        })
```

当出现某种异常类型的时候，你可以返回这个 ApiHTTPError

```
@app.errorhandler(404)
def error_handler_404(e):
    return ApiHTTPError(404, 'page not found')
```

实际上，这种方式和直接返回响应 Response 对象没有什么区别，只是使用 ApiHTTPError, 可以不再定义需要传输的

msg, data 等字段，你可以在 ApiHTTPError 的 get_body 当中统一处理。

具体使用的时候，2种方法都是可以的



```
@app.errorhandler(404)
def error_handler_404(e):
    return {"msg": "not found", "data": ""}, 404
```

## 全局统一错误配置

如果你不想一个一个定义 @app.errorhandler, 可以设置一个全局的错误处理函数。

```
@app.errorhandler(Exception)
def error_handler_all(e):
    if app.config['DEBUG']:
        return e
    if isinstance(e, NotFound):
        return ApiHTTPError(404, "no this page")
    elif isinstance(e, ZeroDivisionError):
        return ApiHTTPError(500, "can not do this")
    elif isinstance(e, ValidationError):
        return ApiHTTPError(401, e.messages)
    return ApiHTTPError(500, "unknown error")
```

这里的 ValidationError 是 marshmallow 的验证错误，e.messages 可以获取序列化失败的原因。

在非 debug 模式下， 验证数据失败，则会得到一个数据校验错误的 json 格式响应。 以下是序列化代码

```
from marshmallow import schema, fields

class UserSchema(schema.Schema):
    username = fields.Str(required=True)
    password = fields.Str()


@app.route('/')
def index():
    user = {}
    resp = UserSchema().load(user)
    return resp
```



中间件：middleware

- 上下文（应用上下文， 请求上下文）
- current_app, g, session, request
- 代理模式（LocalProxy）Request
- 栈数据结构（Stack), 队列，queue, 列表， 先进先出， 后进先出。
- 数据隔离（线程隔离），本地线程代理， Local
- LocalStack

### current_app

[Flask](https://dormousehole.readthedocs.io/en/latest/api.html#flask.Flask) 应用对象 app 具有诸如 [config](https://dormousehole.readthedocs.io/en/latest/api.html#flask.Flask.config) 之类的属性，这些属 性对于在视图或者再命令行调试中访问很有用。但是，在项目中的模 块内导入 app 实例容易导致循环导入问题。

Flask 通过 *应用情境* 解决了这个问题。不是直接引用一个 app ，而是使用

[current_app](https://dormousehole.readthedocs.io/en/latest/api.html#flask.current_app) 代理，该代理指向处理当前活动的应用。

什么是应用情景呢？？？之前讲的上下文，在上下文中访问某些变量才有意义。在上下文外面是没有意义的。

```
from flask import Flask

app = Flask(__name__)

import d3_current_app_view

if __name__ == '__main__':
    app.run(debug=True)
from flask import current_app

from d3_current_app import app

@app.route("/")
def index():
    # 可以通过 current_app 去 访问，避免用 app
    print(current_app.config)
    return 'hello'
```

怎么用的呢？

当 Flask 应用开始处理请求时，它会推送应用情境 和 [请求情境](https://dormousehole.readthedocs.io/en/latest/reqcontext.html) 。当请求结束时，它会在请求情境中弹出，然 后在应用情境中弹出。通常，应用情境将具有与请求相同的生命周期。

app_list = [app1, app2, app3]

新的请求进来，app_list.append(app4), 拿到最后那个app, 处理完了就删除，在处理 app3.

requests 也是一样的。

### session , 跨请求。

有新的 session:



已经存在的 session:



### session 实现

```
from flask import Flask, request, session

app = Flask(__name__)
# 必须要设置 secret_key
app.config['SECRET_KEY'] = 'sfoowff'
# wtf scrf_token


@app.route('/')
def home():
    if not session.get('user'):
        return '你没有登录哦'
    return '这是首页'

# 量子计算
# 不要自己去设计加密算法。

@app.route('/login/')
def login():
    username = request.args.get("username")
    pwd = request.args.get("pwd")
    if username and pwd:
        session['user'] = username + pwd
        return '登录成功'
    return '没有登录'


@app.route('/logout/')
def logout():
    session.pop('user',None)
    return '登出'
```

设置 session 相关：1、永久过期时间 2、cookie 名字



session.permanent=True: 设置才有过期时间，否则浏览器关闭就过期了。

### 随机生成 secret_key

```
scret_key = os.urandom(16)
```

### cookie

```
response = make_response('登录成功')
response.set_cookie('current_time', time.strftime('%Y-%M-%D %H'))
request.cookies.get('current_time')
```

SecureCookieSessionInterface 和 SecureCookieSession

flask 默认提供的 session 功能还是很简单的，满足了基本的功能。但是我们看到 flask 把 session 的数据都保存在客户端的 cookie 中，这里只有用户名还好，如果有一些私密的数据（比如密码，账户余额等等），就会造成严重的安全问题。可以考虑使用 [flask-session](https://pythonhosted.org/Flask-Session/) 这个三方的库，它把数据保存在服务器端（本地文件、redis、memcached），客户端只拿到一个 sessionid。

session 主要是用来在不同的请求之间保存信息，最常见的应用就是登陆功能。虽然直接通过 session 自己也可以写出来不错的登陆功能，但是在实际的项目中可以考虑 [flask-login](https://flask-login.readthedocs.io/en/latest/) 这个三方的插件，方便我们的开发

### g: 同一个请求共享数据

```
# g 变量和 jwt 类似，都是从 _app_ctx_stack 中取对应的属性
# g 变量获取 _app_ctx_stack.top.g
# jwt 获取 _app_ctx_stack.top.jwt
```

1、验证用户信息，

链接数据库的例子：

```
def connect_to_database():
    conn = pymysql.connect(host='localhost',
                                 user='root',
                                 password='',
                                 db='lemon_tester',
                                 charset='utf8mb4')
    return conn.cursor()

@app.before_request
def get_db():
    if 'db' not in g:
        g.db = connect_to_database()

@app.route('/')
def index():
    g.db.execute("/")
    return 'hello'

@app.teardown_request
def teardown_db(exception):
    db = g.pop('db', None)
    if db is not None:
        db.close()
```

视图中使用：

```
@app.route('/register', methods=['GET', 'POST'])
def register():
    g.db.execute('SELECT * FROM user_info;')
    a = g.db.fetchall()
    print(a)
```

注意：before_request 如果返回了值，对应的视图函数不会执行了。直接返回对应的值。 teardown_request 在每次请求中总是会执行。

在一个请求之中共享任何数据。 TON

链接数据库：

```
# 代理模式
from werkzeug.local import LocalProxy
db = LocalProxy(get_db)
```

情景2：表单验证：

### request

当 [Flask](https://dormousehole.readthedocs.io/en/latest/api.html#flask.Flask) 应用处理请求时，它会根据从 WSGI 服务器收到的环境创建一个 [Request](https://dormousehole.readthedocs.io/en/latest/api.html#flask.Request) 对象。因为 *工作者* （取决于服务器的线程，进程或协程）一 次只能处理一个请求，所以在该请求期间请求数据可被认为是该工作者的全局数据。 Flask 对此使用术语 *本地情境* 。

## 请求上下文

- request
- session

## 应用上下文

- current_app
- g

## globals.py 中如何定义的

```
current_app = LocalProxy(_find_app)
request = LocalProxy(partial(_lookup_req_object, "request"))
session = LocalProxy(partial(_lookup_req_object, "session"))
g = LocalProxy(partial(_lookup_app_object, "g"))
```

## stack

栈结构处理多个请求, 先进后出，队列 

![img](https://ky8tjff7tg.feishu.cn/space/api/box/stream/download/asynccode/?code=NWQ4NjM5MzI5ZjgxYTAwNGMyMjY4OThjNDkwYzljZWVfNVN3cEZVWHNHRmNWUkpsT3gxM2xOYkEwblh2Skh1Qm1fVG9rZW46Ym94Y24zMjhKSWtORXZSRFVBU2FrNU9sMDBnXzE2MTcxMDYyNzM6MTYxNzEwOTg3M19WNA)

```
class MyStack:
    def __init__(self, data: list):
        self._data = data

    def push(self, new_elem):
        return self._data.append(new_elem)

    def pop(self):
        return self._data.pop()

    def top(self):
        return self._data[-1]

    def __repr__(self):
        return f'&lt;MyStack {self._data}&gt;'
```

## wsgi_app 源码中如何把请求压入栈

## Local 做线程隔离

- 单请求是排队
- 多请求领号

## LocalStack 结合 Local 和 Stack

从栈结构顶端获取到 请求对象，当即贴上线程标签。

### 请求钩子 before_request

情节1、运行需要某段时间的统计数据，不好去改 视图函数：

```
@app.before_request
def get_url():

    print('统计访问该网址的用户')
```

情节二、验证用户

```
@app.before_request
def get_url():
    user = request.args.get('sign')
    if user is None:
        abort(401)
    g.user = user
```

情节三、请求时间验证

```
@app.before_request
def get_url():
    ts = request.args.get('ts')
    if int(time.time) -  ts &gt; 5:
        abort(412)
```

集中注册：

### @after_request

场景：封装响应信息。server: 'lemon' content-type: ''

```
@app.after_request
# 组装响应对象。
# 当前面出错了这里不会执行
def after(response):
    print('after', request.url)
    response.headers['you'] = 'love'
    return response
```

- response 参数必须有
- return 的必须是一个 Response 对象
- 通常用来修改响应
- *如果视图出现错误，不会调用*

### @app.teardown_request

```
@app.teardown_request
def teardown_request(exception):
    print('teart')
    request.url
```

- 通常用来释放资源
- 不管有没有错误都会调用



## 编写 Flask 程序的 Dockerfile

```
# Dockerfile

# 创建项目目录
RUN mkdir /flask_test_platform
# 指定工作路径
WORKDIR /flask_test_platform

ENV FLASK_APP=run.py
COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt

COPY . .
flask db upgrade

EXPOSE 8000

CMD gunicorn -w 4 -b 0.0.0.0:8000 wsgi:app
#ENTRYPOINT gunicorn -w 4 -b 0.0.0.0:8001 wsgi:app
```

## 编写前端的 Dockerfile

```
FROM nginx:alpine

COPY dist/ /usr/share/nginx/html/
COPY configs/default.conf /etc/nginx/conf.d/
COPY configs/nginx.conf /etc/nginx/nginx.conf

EXPOSE 80 8000 443
```

注意：

- 前端的 api.js 里面设置的 host 要改成 http://192.168.99.102:8000
  - 重新打包： npm run build
- nginx 的代理要转发本地的 8000 请求到 后端的IP 地址： proxy_pass http://192.168.99.102:8000;

## 编写 docker-compose.yaml

```
version: '3'
services:
  platform_flask:
    build: .
    volumes:
    - ".:/flask_code"
    ports:
      - "8000:8000"
  platform_front:
    build: front/
    ports:
      - "80:80"
docker-compose up
```

### docker 等待数据库连接成功再进行其他的操作。

在 [startup order](https://docs.docker.com/compose/startup-order/) 有详细的说明，可以通过 shell 查看能不能正常连接 3306 端口判断。

**但是在实操过程当中无法通过容器名 name:3306 连接对应的 host。**

flask 程序的 Dockerfile 可以这么写：

```
FROM python:3.7-slim
RUN mkdir /testing_platform_flask_backend 
WORKDIR /testing_platform_flask_backend
COPY rqm.txt rqm.txt
RUN pip install -r rqm.txt
COPY . .
EXPOSE 8000
# CMD gunicorn -w 4 -b 0.0.0.0:8000 wsgi:app
# RUN chmod u+x ./entrypoint.sh
ENTRYPOINT ./entrypoint.sh
```

最后调用 entrypoint.sh 执行容器，容器需要等待数据库连接再运行，所以先 sleep 20 :

```
# entrypoint.sh
sleep 20
flask db upgrade
/usr/local/bin/gunicorn -w 4 -b 0.0.0.0:8000 wsgi:app
```

### docker-compose 的 yaml 文件

docker-compose 的设置

```
version: '3'
services:
  db32:
    image: mariadb
    restart: always
    # container_name: db23
    # params used when docker run
    command: --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
    environment:
      - MYSQL_ROOT_PASSWORD=admin123
      - MYSQL_DATABASE=test
      - MYSQL_USER=testuser
      - MYSQL_PASSWORD=testpassword
    volumes:
      - mysql_db:/var/lib/mysql
    ports:
      - "3306:3306"
    # networks: 
    #   - db
    # hostname: 'db'

  backend32:
    depends_on: 
      - db32
    build: ./backend
    ports:
      - "8000:8000"
    # command: ./wait_for_db.sh db:3306 -- ./entrypoint.sh
    # hostname: 'db'
    # networks: 
    #   - db
volumes:
  mysql_db:
# networks:
#   db:
```

volumes 开放数据卷：