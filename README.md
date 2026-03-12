## Django → DRF : Short Notes (Problem → Solution)

### 1. Initial Architecture (Django)

**Facts**

* **Django** originally server-side rendered web apps ke liye design hua tha.
* Browser request bhejta tha → Django view → HTML template render → HTML response.

**Limitation**

* Response mostly **HTML pages** hota tha.

---

### 2. Problem: Mobile & SPA Clients

**Facts**

* Modern clients jaise:

  * mobile apps
  * single-page apps (SPA)

Examples: **React**, **Flutter**

**Requirement**

```
Client → JSON data
```

**Issue**

```
Django → HTML pages
```

Mismatch between **client expectation vs server output**.

---

### 3. Problem: Manual API Development

Django me APIs banana possible tha but repetitive work hota tha.

Typical tasks manually karne padte the:

* JSON conversion
* validation
* authentication
* pagination
* filtering
* CRUD endpoints

**Engineering Issue**

* boilerplate code
* inconsistent API design
* higher chance of bugs.

---

### 4. Problem: Object → JSON Serialization

Database objects ko structured API response me convert karna complex ho sakta tha.

Example pipeline:

```
Django Model
   ↓
Python dict
   ↓
JSON response
```

Relations aur nested data ke case me complexity badh jati thi.

---

### 5. Solution: Django REST Framework

Is problem ko solve karne ke liye **Django REST Framework** develop hua.

Goal:

> Django applications ke liye **standardized REST APIs banana easy banana**.

---

### 6. DRF Key Features

DRF ne kuch major abstractions introduce kiye:

**1️⃣ Serializers**

* Model → JSON conversion
* validation

**2️⃣ API Views / ViewSets**

* REST endpoints simplify karte hain
* CRUD automation

**3️⃣ Built-in API tools**

* authentication
* permissions
* pagination
* filtering
* throttling

**4️⃣ Browsable API**

* browser me API test kar sakte ho.

---

### 7. Modern Architecture

```
Frontend / Mobile App
        ↓
     REST API
        ↓
   Django + DRF
        ↓
      Database
```

 Practically **DRF project = Django project + DRF library**.

---

### Fact

**Django REST Framework** ek **extension library** hai jo **Django** ke upar run karti hai.

Iska matlab:

```
Django (core framework)
        +
Django REST Framework (API tools)
        =
Django project with APIs
```

---

### Project Creation Reality

DRF ke liye **alag project type nahi hota**.

Aap same Django commands use karte ho:

```bash
django-admin startproject projectname
python manage.py startapp api
```

Fir DRF install karte ho:

```bash
pip install djangorestframework
```

Aur `settings.py` me add kar dete ho:

```python
'rest_framework'
```

Bas.

---

### Structural Difference

Difference mainly **code layer** me hota hai.

| Django Web Project | DRF API Project     |
| ------------------ | ------------------- |
| Templates          | JSON responses      |
| Forms              | Serializers         |
| Views              | APIViews / ViewSets |

Lekin **project structure same Django ka hi rehta hai**.

---

### One-line Mental Model

```
DRF is not a framework replacing Django.
DRF is a toolkit for building APIs inside Django.
```

---
## DRF Project Setup (Short Steps)

**Concept:**
DRF project actually **Django project + Django REST Framework library** hota hai.

---

### 1️⃣ Virtual Environment (optional but recommended)

```bash
python -m venv venv
```

Activate:

```bash
venv\Scripts\activate   # Windows
```

---

### 2️⃣ Django Install

```bash
pip install django
```

---

### 3️⃣ Django Project Create

```bash
django-admin startproject projectname
cd projectname
```

Project structure:

```
projectname/
    manage.py
    projectname/
        settings.py
        urls.py
```

---

### 4️⃣ App Create

```bash
python manage.py startapp api
```

---

### 5️⃣ DRF Install

```bash
pip install djangorestframework
```

---

### 6️⃣ `settings.py` me add karo

```python
INSTALLED_APPS = [
    'rest_framework',
    'api',
]
```

---

### 7️⃣ Database migrate karo

```bash
python manage.py migrate
```

---

### 8️⃣ Server run karo

```bash
python manage.py runserver
```

---

## Final Structure

```
projectname/
│
├── projectname/
│   ├── settings.py
│   ├── urls.py
│
├── api/
│   ├── models.py
│   ├── views.py
│   ├── serializers.py
│   ├── urls.py
│
└── manage.py
```

---

✅ **Summary**

```
Create Django project
        ↓
Create app
        ↓
Install DRF
        ↓
Add 'rest_framework' in settings
        ↓
Project ready for API development
```

## Step 1 — Model Create Karna

**Context:**
DRF me bhi models exactly waise hi bante hain jaise **Django** me bante hain. DRF models ko change nahi karta; woh sirf API layer add karta hai.

---

### 1️⃣ `models.py` open karo

Path:

```
api/models.py
```

---

### 2️⃣ Model define karo

Example:

```python
from django.db import models

class Product(models.Model):
    name = models.CharField(max_length=100)
    price = models.IntegerField()
```

---

### 3️⃣ Migrations create karo

Command run karo:

```bash
python manage.py makemigrations
```

Ye Django ko batata hai ki **database schema change hua hai**.

---

### 4️⃣ Database migrate karo

```bash
python manage.py migrate
```

Isse database me table create ho jayega.

---

### 5️⃣ Optional (Admin me show karna)

`api/admin.py`

```python
from django.contrib import admin
from .models import Product

admin.site.register(Product)
```

Ab admin panel me Product model dikhega.

---

### Result

Database me table create ho jayega:

```
Product
-------
id
name
price
```

---

✅ **Summary**

```
models.py → model define
        ↓
makemigrations
        ↓
migrate
        ↓
database table create
```

---
