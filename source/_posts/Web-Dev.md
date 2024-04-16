---

title: Web Dev
date: 2023-07-30 12:04:54
tags: [Web]
categories: [Python]
mathjax: true
---

## Use Flask

```python
from flask import Flask,render_template

app = Flask(__name__)

# establish the connection between '/show/info' and function index()
# while client browses the browser,the browser will run index() automatically
# visit test website '127.0.0.1:5000/show/info' will present the page 'index.html' 
@app.route("/show/info")
def index():
    # Flask will automatically search the html document 'index.html' in folder:'templates'
    return render_template("index.html")

@app.route("/get/news")
def get_news():
    # Flask will automatically search the html document 'news.html' in folder:'templates'
    return render_template("news.html")

if __name__ == '__main__':
    app.run()
```

<!--more-->

Help document: [Flask](https://tutorial.helloflask.com/)

## FrontEnd

### HTML 

```html
<a href="https://github.com/Uestc-Young" target="_blank">
        <img src="/static/Profile_picture.jpg" style="height: 150px;">
</a>
```

While using `Flask` to show our own pictures in the web,we must create a folder called `static` and put pictures in it.
Then the picture address `src` should be your relative address.

```html
<table border="1">
        <thead>
            <tr>
                <th>ID</th>
                <th>Name</th>
                <th>Age</th>
            </tr>
        </thead>
        
        <tbody>
            <tr>
                <td>001</td> 
                <td>Peter</td> 
                <td>18</td>
            </tr>
            <tr>
                <td>002</td> 
                <td>Xv2211z</td> 
                <td>19</td>
            </tr>
            <tr>
                <td>003</td> 
                <td>Shino</td> 
                <td>17</td>
            </tr>
        </tbody>
    </table>
```

```html
<input type="radio" name="1">MALE
<input type="radio" name="1">FAMALE
<input type="checkbox">
<input type="button" value="Log in">
<!--Simple button-->
<input type="submit" value="Log in">
<!--Submit form-->
<select>
    <option> Genshin </option>
    <option> StarRailway</option>
</select>
```

Tags in `form`:

```html
input select textarea
```

And tags should have their name which present in the url.

```html
<form method="get" action="">
    <!--method:get/post action:submit address-->
        <div>
            User Name:<input type="text" name="username">
        </div>
        <div>
            Password:<input type="password" name="pwd">
        </div>
        <div>
            <input type="submit" value="Submit">
        </div>
    </form>
```

**Example: User sign up**

```python
from flask import Flask,render_template,request

app = Flask(__name__)

@app.route('/register',methods = ["GET","POST"])
def register():
    if request.method == "GET":
        return render_template('register.html')
    else:
        print(request.form)
        return 'Sign up successfully by HTTP POST!'

if __name__=='__main__':
    app.run()
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>User Register</title>
</head>
<body>
    <h1>Sign up</h1>
    <form method="post" action="/register">
    <!--method:get/post action:submit address-->
        <div>
            User Name:
            <input type="text" name="username">
        </div>
        <div>
            Password:
            <input type="password" name="pwd">
        </div>
        <div>
            Gender:
            <input type="radio" name="gender" value="1"> Male 
            <input type="radio" name="gender" value="2"> Female
        </div>
        <div>
            Hobby:
            <input type="checkbox" name="hobby" value="1">Play Games
            <input type="checkbox" name="hobby" value="2">Watch tiktok
            <input type="checkbox" name="hobby" value="3">Play soccer
            <input type="checkbox" name="hobby" value="4">Reading books
        </div>
        <div>
            Address:
            <select name="city">
                <option value="cd"> Chengdu </option>
                <option value="hf"> Hefei </option>
                <option value="la"> Los Angelos</option>
                <option value="l"> London </option>
            </select>
        </div>
        <div>
            <input type="submit" value="Submit">
        </div>
    </form>
</body>
</html>
```

### CSS

CSS files should be put in `static` in Flask.

```html
<link rel="stylesheet" href="/static/commons.css">
```

#### CSS Selector

- ID Selector

```css
#x{

}
```

- Class Selector

```css
.x{

}
```

- Tag Selector

```css
li{

}
```

- Attribute Selector

```css
.vx[type = 'text']{

}
```

- Descendant Selector

Find all elements which satisfies the selector. 

```css
.vx li{
    
}
```

- Child Selector

Find only the elements which satisfies the selector in the direct child. 

```css
.vx > li{
    
}
```

#### CSS Style

`Inline tags and Block tags`

Use CSS `display:inline-block` to change inline tags to some degree like block tags.

```css
,x {
    display:inline-block;
    height:100px;
    width:100px;
}
```



```css
.x{
    height:100px;
    width:50px;
    font-size:58px;
    font-weight: 600;
    text-align:center;/* horizontal align */ 
    line-height:59px;/* vertical align */
}
```

`margin and padding` 

```css
.x {
    margin: 20px;
    padding: 20px;
}
```

`margin` decides the outer distance of the div.

`padding` decides the inner distance of the div.

`position`

Use `position` to adjust the position of the pattern.

`fixed` 

used to fix the pattern of the whole page.

```css
.x{
    position:fixed
}
```

`relative and absolute`

These two styles are often used together to put the pattern in a specific way.

For example:

```html
.c1 {
    width: 100px;
    height: 200px;
    border: 1px solid red;
    position: relative;
}

.c2 {
    width: 50px;
    height: 50px;
    position: absolute;
    background-color: aqua;
    right: 0;
    bottom: 0;
}
<div class="c1">
        <div class="c2"></div>
</div>
```

#### Bootstrap

**Official Chinese document ：[Bootstrap](https://v3.bootcss.com/)**

Download in your own file and use it following the official document!

```html
<link rel="stylesheet" href="static/plugins/bootstrap-3.4.1/css/bootstrap.css">
```

A simple example:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <link rel="stylesheet" href="static/plugins/bootstrap-3.4.1/css/bootstrap.css">
    <style>
        .c1{
            width: 500px;
            height: 300px;
            background-color:rgb(254, 254, 254);
            margin: 150px auto;
            padding: 10px 20px;
            border: 1px solid #ddd;
            box-shadow: 5px 5px 20px #aaa;
        }

        .login{
            text-align: center;
        }

    </style>
</head>
<body>
    <div class="c1">
        <form class="form">
            <div>
                <h1 class="login">Log in</h1>
            </div>
            <div class="form-group">
              <label for="exampleInputEmail1">Email address</label>
              <input type="email" class="form-control" id="exampleInputEmail1" placeholder="Email">
            </div>
            <div class="form-group">
              <label for="exampleInputPassword1">Password</label>
              <input type="password" class="form-control" id="exampleInputPassword1" placeholder="Password">
            </div>
            <button type="submit" class="btn btn-success">Submit</button>
          </form>
    </div>
</body>
</html>
```

### JavaScript

A simple example up down here.

Usually when using `javascript`, go to the website and find how to use it.( 

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <input type="text" placeholder="Please enter the city" id="city">
    <input type="button" value="Submit" onclick="fun()">
    <ul id="list">
    </ul>

    <script type="text/javascript">
        function fun(){
            var city = document.getElementById("city").value;
            var tag = document.createElement("li");
            tag.innerText = city;
            if (city.length > 0){
                var parent = document.getElementById("list");
                parent.appendChild(tag);
                document.getElementById("city").value="";
            }else{
                alert("Error!");
            }
        }
    </script>
</body>
</html>
```

#### JQuery

`jQuery` is one of the modules of `JavaScript`.

Download or use CDN link to use it.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="https://apps.bdimg.com/libs/jquery/2.1.4/jquery.min.js">
    </script>
</head>
<body>
    <div id="1">
        This is a new world.
    </div>

    <script type="text/javascript">
        $("#1").text("Hello world")
    </script>
</body>
</html>
```

**Attention**, in order to avoid loading `jQuery` code before loading the whole html page. it is recommended to write the `jQuery` code here⬇️

```javascript
$(function(){
 	// Start to write jQuery code...
});
```

Else, we can use some specific character like `closure` etc. in javascript.

The way how `jQuery` choose elements is the same as `CSS`. 

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="https://apps.bdimg.com/libs/jquery/2.1.4/jquery.min.js">
    </script>
</head>
<body>
   <div>
        <div>
            <div>Paris</div>
            <div id="c1">London</div>
            <div>New York</div>
        </div>
   </div>

   <script type="text/javascript">
        var tmp = $("#c1");
        console.log(tmp);
        console.log(tmp.parent());
        console.log(tmp.prev());
   </script>
</body>
</html>
```

See the code above, 

we can use `parent()` to find the parent and `children()` to find **all the children tags**. 

Also, we can use `prev()` and `next()` to find the brother tags.

We can also use `find()` to find the specific tags.

Eg, `find(".c1")` is to find all the descendant tags which class equal to `c1`.

Simple example, Side menu:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .leftmenu{
            height: 800px;
            width: 200px;
            border: 1px solid red;
        }
        .head{
            background-color: gold;
        }
        .content a{
            display: block;
            padding: 5px;
        }
        .hide{
            display: none;
        }
    </style>
    <script src="https://apps.bdimg.com/libs/jquery/2.1.4/jquery.min.js">
    </script>
    <script>
        function clickMe(self){
            $(self).next().removeClass("hide");
            $(self).parent().siblings().find(".content").addClass("hide");
        }
    </script>
</head>
<body>
    <div class="leftmenu">
        
        <div class="item">
            <div class="head" onclick="clickMe(this);">ShangHai</div>
            <div class="content hide">
                <a>Hongqiao Airport</a>
                <a>Pudong Airport</a>
            </div>
        </div>
        
        <div class="item">
            <div class="head" onclick="clickMe(this);">BeiJing</div>
            <div class="content hide">
                <a>Daxin Airport</a>
                <a>Capital Airport</a>
            </div>
        </div>

        <div class="item">
            <div class="head" onclick="clickMe(this);">ChengDu</div>
            <div class="content hide">
                <a>Tianfu Airport</a>
                <a>ShuangLiu Airport</a>
            </div>
        </div>

    </div>

</body>
</html>
```

### Vue

```
Keep learning...👋
```

## BackEnd

### PostgreSQL

Python driver: `psycopg2`

```shell
pip install psycopg2
```

Connect to server:

```python
import psycopg2

conn = psycopg2.connect(database="db_name", user="postgres", password="pwd", host="127.0.0.1", port="5432")
```

Create cursor:

```python
cursor=conn.cursor()

cursor.execute('''create table public.player(
id integer not null primary key,
name varchar(32) not null,
height decimal(5, 2) not null,
weight decimal(5, 2) not null
)''')

conn.commit()
```

Close cursor and connection:

```python
cursor.close()
conn.close()
```

**Simple try on: 👉 Flask + PostgreSQL**

```python
from flask import Flask,render_template,request
import psycopg2

app = Flask(__name__)

@app.route('/add/user' , methods = ['POST','GET'])
def add_user():
    if request.method == 'GET':
        return render_template("add_user.html")
    else:
        username = request.form.get("username")
        password = request.form.get("pwd")

        conn = psycopg2.connect(database="flaskdemo", user="postgres", password="pwd", host="127.0.0.1", port="5432")

        cursor=conn.cursor()

        sql = "insert into users(username,pwd) values(%s,%s)"

        cursor.execute(sql,[username,password])

        conn.commit()
        
        cursor.close()
        conn.close()

        return "Successfully append!"

@app.route('/query/users' , methods = ['POST','GET'])
def query_users():
    conn = psycopg2.connect(database="flaskdemo", user="postgres", password="pwd", host="127.0.0.1", port="5432")

    cursor = conn.cursor()

    sql = "select * from users;"

    cursor.execute(sql)

    datalist = cursor.fetchall()
    
    datalist_dict = [{'username': data[0], 'pwd': data[1]} for data in datalist]
    
    cursor.close()
    conn.close()


    return render_template('query_users.html',datalist = datalist_dict)

if __name__ == '__main__':
    app.run()
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>user</title>
</head>
<body>
    <h1>Add Users</h1>
    <form method="post">
        <div>
            <input type="text" placeholder="Username" name="username">
        </div>
        <div>
            <input type="password" placeholder="Password" name="pwd">
        </div>
        <input type="submit" value="Submit">
    </form>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>user query</title>
</head>
<body>
    <table border="1">
        <thead>
            <tr>
                <th>Username</th>
                <th>Password</th>
            </tr>
        </thead>
        <tbody>
            {% for data in datalist %}
            <tr>
                <th>{{data.username}}</th>
                <th>{{data.pwd}}</th>
            </tr>
            {% endfor %}
        </tbody>
    </table>
</body>
</html>
```

### Django

**Django Official document: [Django](https://docs.djangoproject.com/zh-hans/4.2/)**

Install: 

```shell
pip install django
```

Create a new project:

```shell
django-admin startproject project_name
```

Run server:

```shell
python manage.py runserver (port)
```

Create an app:

```shell
python manage.py startapp app_name 
```

Then we should edit `app_name/urls.py` ,`project_name/urls.py` and `app_name/views.py`.

First, write the pages in `app_name/views.py`, like this, and the html file should be put in `templates` which is put in the app folder.

```python
from django.http import render

def index(request):
    return render(request,"xxx.html")
```

Then we should edit the `app_name/urls.py`:

```python
from . import views

urlpatterns = [
    path("", views.index, name="index"),
]
```

Finally,edit  `project_name/urls.py` so that django can find the path in the whole project.

```python
from django.urls import include, path

urlpatterns = [
    path("app_name/", include("app_name.urls")),
    path("admin/", admin.site.urls),
]
```

#### Connect to DataBase

I choose to use `PostgreSQL`  in my  hands on project, and the first step is to change the `settings.py` in the `project_name` dir.

 Then add your app into `settings.py` as well. Like this⬇️

```python
INSTALLED_APPS = [
    "polls.apps.PollsConfig",	# this is what I have add into settings
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
]
```

Finally, three more steps are needed:

- Edit models.py ,create models we need.

```python
from django.db import models

class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateField("date published")

class Choice(models.Model):
    question = models.ForeignKey(Question,on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
```

- ```python
  python manage.py makemigrations

- ```python
  python manage.py migrate
  ```

  
