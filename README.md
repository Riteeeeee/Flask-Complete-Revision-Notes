# 🚀 Flask Complete Revision Notes

A **detailed Flask revision guide** for backend development and SDE interviews.

This guide covers:

* Flask basics
* Routing
* Templates
* Request & Response handling
* Database integration
* Sessions & cookies
* REST APIs
* Project structure
* Best practices
* Interview questions

---

# 📌 What is Flask?

Flask is a **lightweight web framework for Python** used to build:

* Web applications
* REST APIs
* Microservices
* Backend services for ML models

Flask follows the **WSGI standard** and is called a **micro-framework** because it does not enforce a strict project structure.

### Key Features

* Minimal and flexible
* Built-in development server
* Jinja2 template engine
* REST API friendly
* Easy to learn
* Extensible via plugins

---

# ⚙️ Installing Flask

### Install Flask

```bash
pip install flask
```

### Create Virtual Environment

```bash
python -m venv venv
```

Activate environment

**Linux / Mac**

```bash
source venv/bin/activate
```

**Windows**

```bash
venv\Scripts\activate
```

---

# 🧱 Basic Flask Application

Example of a minimal Flask application:

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def home():
    return "Hello Flask!"

if __name__ == "__main__":
    app.run(debug=True)
```

### Explanation

| Component       | Meaning                   |
| --------------- | ------------------------- |
| Flask(**name**) | Creates Flask application |
| @app.route()    | Maps URL to function      |
| return          | Sends response to client  |
| debug=True      | Enables debugging         |

---

# 🌐 How Flask Works (Architecture)

```
Client (Browser/Postman)
        ↓
HTTP Request
        ↓
Flask Server
        ↓
Route Handler
        ↓
Business Logic
        ↓
Database / Services
        ↓
Response Returned
        ↓
Client
```

---

# 🗂 Flask Project Structure

Small project:

```
flask-app
│
├── app.py
├── templates
│   └── index.html
├── static
│   ├── css
│   ├── js
│   └── images
└── requirements.txt
```

Large production project:

```
project
│
├── app
│   ├── __init__.py
│   ├── routes.py
│   ├── models.py
│   ├── services.py
│
├── templates
├── static
├── config.py
├── run.py
└── requirements.txt
```

---

# 🌍 Routing in Flask

Routing connects **URL endpoints to Python functions**.

### Basic Routing

```python
@app.route("/about")
def about():
    return "About Page"
```

### Dynamic Routing

```python
@app.route("/user/<name>")
def user(name):
    return f"Hello {name}"
```

Example URL

```
http://localhost:5000/user/Ritee
```

Output

```
Hello Ritee
```

### Multiple HTTP Methods

```python
@app.route("/login", methods=["GET", "POST"])
def login():
    return "Login endpoint"
```

---

# 📨 HTTP Methods

Flask supports all HTTP methods:

| Method | Purpose             |
| ------ | ------------------- |
| GET    | Retrieve data       |
| POST   | Create new resource |
| PUT    | Update resource     |
| DELETE | Delete resource     |
| PATCH  | Partial update      |

Example:

```python
@app.route("/api/user", methods=["POST"])
def create_user():
    return "User created"
```

---

# 📦 Templates (Jinja2)

Flask uses **Jinja2 template engine** to render HTML.

### Example Template

`templates/index.html`

```html
<h1>Hello {{ name }}</h1>
```

### Rendering Template

```python
from flask import render_template

@app.route("/")
def home():
    return render_template("index.html", name="Ritee")
```

### Jinja2 Features

* Variables
* Loops
* Conditions
* Template inheritance

Example loop

```html
<ul>
{% for user in users %}
<li>{{ user }}</li>
{% endfor %}
</ul>
```

---

# 📁 Static Files

Static files include:

* CSS
* JavaScript
* Images

Example:

```
static/css/style.css
```

HTML usage

```html
<link rel="stylesheet" href="{{ url_for('static', filename='css/style.css') }}">
```

---

# 📥 Request Object

Used to read **client request data**.

```python
from flask import request
```

### Access form data

```python
request.form["username"]
```

### Access query parameters

```
/search?q=flask
```

```python
request.args.get("q")
```

### Access JSON data

```python
data = request.json
```

---

# 📤 Response Object

Flask automatically converts return values into HTTP responses.

Example:

```python
return "Hello"
```

Returning JSON:

```python
from flask import jsonify

@app.route("/api")
def api():
    return jsonify({
        "status": "success"
    })
```

---

# 🔄 Redirects

Used to redirect users to another endpoint.

```python
from flask import redirect, url_for

@app.route("/home")
def home():
    return redirect(url_for("login"))
```

---

# 🍪 Cookies

Cookies store small data on **client browser**.

Example:

```python
from flask import make_response

@app.route("/cookie")
def cookie():
    resp = make_response("Cookie set")
    resp.set_cookie("username", "Ritee")
    return resp
```

---

# 🧠 Sessions

Sessions store data **on the server side**.

```python
from flask import session

app.secret_key = "secret"

@app.route("/set")
def set_session():
    session["user"] = "Ritee"
    return "Session set"
```

---

# 🗄 Database Integration

Flask supports multiple databases.

### SQLite

```python
import sqlite3

conn = sqlite3.connect("database.db")
cursor = conn.cursor()
cursor.execute("SELECT * FROM users")
```

---

# 🧠 Flask SQLAlchemy (ORM)

SQLAlchemy allows interaction with databases using **Python objects instead of SQL queries**.

### Install

```bash
pip install flask-sqlalchemy
```

### Example

```python
from flask_sqlalchemy import SQLAlchemy

db = SQLAlchemy(app)

class User(db.Model):

    id = db.Column(db.Integer, primary_key=True)

    name = db.Column(db.String(100))

    email = db.Column(db.String(100))
```

Create table

```python
db.create_all()
```

---

# 🔌 Flask Extensions

Flask has many extensions.

| Extension        | Purpose             |
| ---------------- | ------------------- |
| Flask-SQLAlchemy | Database ORM        |
| Flask-Login      | User authentication |
| Flask-Migrate    | Database migration  |
| Flask-RESTful    | Build REST APIs     |
| Flask-WTF        | Forms               |

---

# 🔐 Debug Mode

Debug mode enables:

* Auto reload
* Detailed error messages

```python
app.run(debug=True)
```

⚠️ Never enable debug mode in production.

---

# 🧩 Flask Blueprints

Blueprints help organize large Flask apps.

Example:

```
auth blueprint
product blueprint
order blueprint
```

Example:

```python
from flask import Blueprint

auth = Blueprint("auth", __name__)

@auth.route("/login")
def login():
    return "Login Page"
```

---

# 🚀 Creating REST API in Flask

Example API:

```python
@app.route("/api/users", methods=["GET"])
def get_users():
    return jsonify([
        {"name":"Alice"},
        {"name":"Bob"}
    ])
```

Example POST API:

```python
@app.route("/api/users", methods=["POST"])
def add_user():
    data = request.json
    return jsonify(data)
```

---

# ⚡ Flask vs Django

| Feature        | Flask                | Django         |
| -------------- | -------------------- | -------------- |
| Type           | Micro Framework      | Full Framework |
| Flexibility    | Very high            | Moderate       |
| Learning curve | Easy                 | Moderate       |
| Best for       | APIs & microservices | Large apps     |

---

# 🧠 Flask Interview Questions

Most frequently asked:

1. What is Flask?
2. What is WSGI?
3. What is Jinja2?
4. What are Blueprints?
5. Flask vs Django?
6. What are cookies?
7. What are sessions?
8. What is Flask SQLAlchemy?
9. How does routing work?
10. How to create REST APIs?

---

# ⭐ Best Use Cases of Flask

Flask is ideal for:

* REST APIs
* Microservices
* ML model APIs
* Backend services
* Lightweight web applications

---

# 🎯 Interview Tip

Explain Flask architecture like this:

```
Client
   ↓
Request
   ↓
Flask App
   ↓
Route Handler
   ↓
Business Logic
   ↓
Database
   ↓
Response
   ↓
Client
```

---

# 📚 References

Official Documentation

https://flask.palletsprojects.com/
