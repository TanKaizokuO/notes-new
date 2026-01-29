---
tags:
  - main
---
---

# Part 1: List and dict comprehensions

```python
fruits = ["Apples", "Bananas", "Pears"]
book1 = {"title": "Great Expectations", "author": "Charles Dickens"}
book2 = {"title": "Bleak House", "author": "Charles Dickens"}
book3 = {"title": "An Book By No Author"}
book4 = {"title": "Moby Dick", "author": "Herman Melville"}
books = [book1, book2, book3, book4]
```

```python 
fruits_shouted = []
for fruit in fruits:
	fruits_shouted.append(fruit.upper())

fruits_shouted
```
**OUTPUT** : ['APPLES', 'BANANAS', 'PEARS']

```PYTHON
# But you may not know that you can do this to create dictionaries, too:
fruit_mapping = {fruit: fruit.upper() for fruit in fruits}
fruit_mapping
```
**OUTPUT** : {'Apples': 'APPLES', 'Bananas': 'BANANAS', 'Pears': 'PEARS'}

---

```PYTHON
# you can also use the if statement to filter the result
fruits_with_longer_names_shouted = [fruit.upper() for fruit in fruits if len(fruit)>5]
fruits_with_longer_names_shouted
```
**OUTPUT** : ['APPLES', 'BANANAS']

---


```PYTHON
# This code will fail with an error because one of our books doesn't have an author
[book['author'] for book in books] 
```
**OUTPUT** : ERROR


```PYTHON
# But this will work, because get() returns None
[book.get('author') for book in books]
```
**OUTPUT** : ['Charles Dickens', 'Charles Dickens', None, 'Herman Melville']


```python
# And this variation will filter out the None
[book.get('author') for book in books if book.get('author')]
```
**OUTPUT** : ['Charles Dickens', 'Charles Dickens', 'Herman Melville']

---


```PYTHON
# And this version will convert it into a set, removing duplicates
set([book.get('author') for book in books if book.get('author')])
```
**OUTPUT** : {'Charles Dickens', 'Herman Melville'}

---


```PYTHON
# curly braces creates a set, so this is a set comprehension
{book.get('author') for book in books if book.get('author')}
```
**OUTPUT** :{'Charles Dickens', 'Herman Melville'}

---
---
# Part 2: Generators

```PYTHON
# First define a generator; it looks like a function, but it has yield instead of return

import time
def come_up_with_fruit_names():
	for fruit in fruits:
		time.sleep(1) # thinking of a fruit
		yield fruit

# Then use it
for fruit in come_up_with_fruit_names():
  print(fruit)
```
**OUTPUT** : 

Apples
Bananas
Pears

---

```PYTHON
# Here's the same thing written with list comprehension
def authors_generator():
	for author in [book.get("author") for book in books if book.get("author")]:
		yield author
		
# Use it
for author in authors_generator():
	print(author)
```
**OUTPUT** :

Charles Dickens
Charles Dickens
Herman Melville

---

```PYTHON
# Here's a nice shortcut
# You can use "yield from" to yield each item of an iterable
def authors_generator():
	yield from [book.get("author") for book in books if book.get("author")]
```
**OUTPUT** :
Charles Dickens
Charles Dickens
Herman Melville

---
# Part 3 : Advanced Class Concepts 
### 1. Type Hints in Class Variables

In Python, type hints help specify the expected type of a variable, which makes code more readable and assists IDEs and linters in catching type-related issues. They’re especially useful in larger codebases where it’s helpful to know the types of variables that a class should hold.

```PYTHON
from typing import Optional

class Book:
    title: str
    author: Optional[str] = None
    year_published: Union[int, str]  # year could be 'Unknown' or an int

    def __init__(self, title: str, author: str):
        self.title = title
        self.author = author

```
---

### 2. Subclasses

Subclasses allow you to create a class that inherits attributes and methods from another class. This is a form of polymorphism, where the subclass can either inherit or override behavior from the parent (or "superclass"). It’s useful when you want to build on existing functionality.

Here's an example using `Book` as the base class:

```python
class Book:     
	def __init__(self, title: str, author: str):
		self.title = title         
		self.author = author      
	def get_description(self) -> str:         
		return f"'{self.title}' by {self.author}"  
		
# Subclass for eBooks 
class EBook(Book):     
	def __init__(self, title: str, author: str, file_format: str):         
			super().__init__(title, author)  # Initialize attributes from Book
			self.file_format = file_format      
	def get_description(self) -> str:         
		# Overriding the parent class method         
		return f"'{self.title}' by {self.author} (format: {self.file_format})"
```
In this example:

- `EBook` is a subclass of `Book`.    
- We call `super().__init__(title, author)` to initialize the title and author properties from the `Book` class.
- `EBook` also adds a new attribute, `file_format`, and overrides `get_description()` to include information about the file format.
- Here -> is the expected data type.

Subclasses can override methods in the superclass to provide specific functionality, as shown with `get_description()`.

---
### 3. Class Methods

Class methods are methods that belong to the class itself, rather than any instance of the class. They are defined using the `@classmethod` decorator and take `cls` (representing the class) as their first parameter instead of `self` (representing the instance). Class methods are commonly used for factory methods—alternative constructors that return a new instance of the class with specific attributes set.

```PYTHON
from typing import List

class Book:
    title: str
    author: str

    def __init__(self, title: str, author: str):
        self.title = title
        self.author = author

    def get_description(self) -> str:
        return f"'{self.title}' by {self.author}"

    @classmethod
    def create_multiple(cls, titles_and_authors: List[tuple]) -> List['Book']:
        """Class method that returns a list of Book instances from a list of title-author pairs."""
        return [cls(title, author) for title, author in titles_and_authors]

```

In this example:

- `create_multiple` is a class method, indicated by the `@classmethod` decorator.
- It takes a list of title-author pairs and creates a list of `Book` instances.
- We use `cls` instead of `self`, meaning `cls` will refer to `Book` when this method is called on `Book`.
To use the `create_multiple` class method:

```python
books = Book.create_multiple([("1984", "George Orwell"), ("Brave New World", "Aldous Huxley")])
for book in books:
    print(book.get_description())

```
**OUTPUT** :
'1984' by George Orwell
'Brave New World' by Aldous Huxley

---