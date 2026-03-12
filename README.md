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

## Step 2 — Serializer Banana

**Context:**
Ab model database me ban gaya. Next step me hume **model data ko API response (JSON)** me convert karna hota hai.

Ye kaam **Django REST Framework** me **Serializer** karta hai.

Simple words me:

```text
Model Object  ↔  Serializer  ↔  JSON
```

---

## 1️⃣ `serializers.py` file create karo

App folder me new file banao:

```text
api/serializers.py
```

---

## 2️⃣ Serializer define karo

Example:

```python
from rest_framework import serializers
from .models import Product


class ProductSerializer(serializers.ModelSerializer):
    
    class Meta:
        model = Product
        fields = '__all__'
```

---

## 3️⃣ Code samjho (short)

### `ModelSerializer`

Ye automatically:

* model fields read karta hai
* JSON conversion karta hai
* validation handle karta hai

---

### `Meta class`

```python
class Meta:
    model = Product
    fields = '__all__'
```

Meaning:

| Line   | Meaning                       |
| ------ | ----------------------------- |
| model  | kaunsa model use hoga         |
| fields | kaunse fields API me dikhenge |

Example response:

```json
{
  "id": 1,
  "name": "Laptop",
  "price": 50000
}
```

---

## Result

Ab system ready hai:

```text
Database Model
      ↓
Serializer
      ↓
JSON data
```

Lekin abhi **API endpoint nahi bana**.

---

✅ **Summary**

```text
serializers.py create
        ↓
ModelSerializer define
        ↓
Model data → JSON conversion ready
```

---

Ab hum **Serializer** ko systematically samjhenge. Yeh **Django REST Framework** ka core component hai.

Main isko structured way me explain karunga:

1. Serializer kya hota hai
2. Basic Serializer
3. ModelSerializer
4. Default Fields
5. Field options
6. Custom Serializer
7. Validation

---

# 1️⃣ Serializer kya hota hai

**Fact**

Serializer ka main kaam:

```
Complex Data (Model instances / QuerySets)
            ↓
JSON
```

Aur reverse bhi:

```
JSON input
     ↓
Python objects
     ↓
Model instance
```

Is process ko **serialization / deserialization** kehte hain.

Pipeline:

```
Model instance
      ↓
Serializer
      ↓
JSON response
```

---

# 2️⃣ Basic Serializer (Manual Serializer)

Ye lowest-level serializer hota hai.

Example:

```python
from rest_framework import serializers

class ProductSerializer(serializers.Serializer):
    id = serializers.IntegerField()
    name = serializers.CharField(max_length=100)
    price = serializers.IntegerField()
```

Yahan:

* fields manually define karne padte hain
* model se automatic mapping nahi hoti

---

### Example Data

Model instance:

```
Product(id=1, name="Laptop", price=50000)
```

Serializer output:

```json
{
 "id":1,
 "name":"Laptop",
 "price":50000
}
```

---

# 3️⃣ ModelSerializer (Most Used)

DRF me most commonly use hota hai **ModelSerializer**.

Example:

```python
class ProductSerializer(serializers.ModelSerializer):

    class Meta:
        model = Product
        fields = "__all__"
```

Ye automatically:

* model fields detect karta hai
* serializer fields generate karta hai
* validation add karta hai

---

## Internally kya hota hai

Agar model ho:

```python
class Product(models.Model):
    name = models.CharField(max_length=100)
    price = models.IntegerField()
```

DRF internally generate karta hai:

```python
name = serializers.CharField(max_length=100)
price = serializers.IntegerField()
```

---

# 4️⃣ Default Serializer Fields

DRF me built-in fields available hain.

Common ones:

| Field         | Use                |
| ------------- | ------------------ |
| CharField     | text               |
| IntegerField  | integer            |
| BooleanField  | true/false         |
| FloatField    | decimal numbers    |
| EmailField    | email validation   |
| URLField      | URLs               |
| DateField     | date               |
| DateTimeField | timestamp          |
| ChoiceField   | predefined choices |

Example:

```python
class ExampleSerializer(serializers.Serializer):

    name = serializers.CharField(max_length=100)
    age = serializers.IntegerField()
    email = serializers.EmailField()
```

---

# 5️⃣ Field Options (Important)

Serializer fields me multiple options hote hain.

Example:

```python
name = serializers.CharField(
    max_length=100,
    required=True,
    allow_blank=False
)
```

Common options:

| Option      | Meaning                    |
| ----------- | -------------------------- |
| required    | field required hai ya nahi |
| read_only   | sirf response me dikhega   |
| write_only  | sirf input me use hoga     |
| default     | default value              |
| allow_null  | null allow karega          |
| allow_blank | empty string allow karega  |

Example:

```python
price = serializers.IntegerField(required=True)
```

---

# 6️⃣ Custom Fields (Override)

Kabhi kabhi model field ko modify karna padta hai.

Example:

```python
class ProductSerializer(serializers.ModelSerializer):

    name = serializers.CharField(max_length=200)

    class Meta:
        model = Product
        fields = "__all__"
```

Yahan model me agar `max_length=100` bhi ho
serializer usko override kar sakta hai.

---

# 7️⃣ Validation

Serializer validation handle karta hai.

Example:

```python
class ProductSerializer(serializers.ModelSerializer):

    class Meta:
        model = Product
        fields = "__all__"

    def validate_price(self, value):

        if value < 0:
            raise serializers.ValidationError("Price cannot be negative")

        return value
```

Yeh **field level validation** hai.

---

# 8️⃣ Object Level Validation

Multiple fields validate karne ke liye:

```python
def validate(self, data):

    if data["price"] < 10:
        raise serializers.ValidationError("Price too low")

    return data
```

---

# 9️⃣ Serializer ka Actual Use

Example:

```python
product = Product.objects.first()

serializer = ProductSerializer(product)

serializer.data
```

Output:

```json
{
 "id":1,
 "name":"Laptop",
 "price":50000
}
```

---

# Mental Model (Important)

```
Database Model
      ↓
Serializer
      ↓
JSON Response
```

Aur reverse:

```
JSON Request
      ↓
Serializer
      ↓
Model instance
```

---

✅ **Short Summary**

| Concept    | Meaning                     |
| ---------- | --------------------------- |
| Serializer | data convert karta hai      |
| Serializer | validation handle karta hai |
| Serializer | JSON ↔ Python conversion    |

---
