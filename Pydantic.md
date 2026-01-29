---
tags:
  - main
---
---
## What is Pydantic?

Pydantic is a **data validation and settings management** library using Python type annotations. 
It’s built around the concept of defining **schemas** as Python classes. These schemas ensure the data **matches the expected types**, and optionally transforms or validates the data automatically.

## BaseModel: Your Schema Blueprint

All Pydantic models inherit from `pydantic.BaseModel`. When you create a subclass of `BaseModel`, you define a data **schema**.

```python
from pydantic import BaseModel

class Book(BaseModel):
    title: str
    author: str
    pages: int
    published: bool = True  # Default value

book = Book(title="1984", author="George Orwell", pages=328)
print(book)

```

**OUTPUT :** title='1984' author='George Orwell' pages=328 published=True

---

 ```python
book = Book(title="Dune", author="Frank Herbert", pages="412")
print(book.pages) 
 ```
We see that string "412" has been automatically converted into int.

```python
Book(title="Dune", author="Frank Herbert", pages="not_a_number")
```
 Validation error for Book page

---

## Instantiating with **<Dictionary_name>

```python
fields = {
    "title": "The Hobbit",
    "author": "J.R.R. Tolkien",
    "pages": 310
}

book = Book(**fields)
print(book)
```

---

# From and To JSON

use  **model_dump()** for dict representation

```python
print(book.model_dump())
```
**OUTPUT :**
```python
{'title': 'The Hobbit', 'author': 'J.R.R. Tolkien', 'pages': 310, 'published': True}
```

use  **model_dump_json()**  to convert your model to a JSON string:
```python
print(book.model_dump_json())

```
Or use the standard `json` module:
```python
import json 
json_string = json.dumps(book.model_dump()) 
print(json_string)
```
**OUTPUT :**
```python
{"title":"Dune","author":"Frank Herbert","pages":412,"published":true}
```

Loading from JSON

```python
json_string = '{"title": "Frankenstein", "author": "Mary Shelley", "pages": 280}'
data = json.loads(json_string)
book = Book(**data)
book
```
**OUTPUT :**
```python
Book(title='Frankenstein', author='Mary Shelley', pages=280, published=True)
```

---

# Nested Models
```python
class Author(BaseModel):
    name: str
    birth_year: int

class Book(BaseModel):
    title: str
    author: Author
    pages: int

a = Author(name="Isaac Asimov", birth_year=1920)
b = Book(title="Foundation", author=a, pages=255)
print(b)
```

**OUTPUT :**
```python
title='Foundation' author=Author(name='Isaac Asimov', birth_year=1920) pages=255
```

```python
data = {
    "title": "Neuromancer",
    "author": {
        "name": "William Gibson",
        "birth_year": 1948
    },
    "pages": 271
}

b = Book(**data)
print(b)
```

**OUTPUT :**
```python
title='Foundation' author=Author(name='Isaac Asimov', birth_year=1920) pages=255
```

---

