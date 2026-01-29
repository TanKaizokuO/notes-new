---
tags:
  - main
---
---

# F strings
**Formatting Numbers** : .2f show only two decimal points 
```python
w=2.056
b=5.678
example = f"Area = {w:,.2f} * {b:,.2f}"
```

---

# File Handling
use **with...open** format as it automatically closes files

```python
with open("<filename>,<open-mode>") as file:
```
- for file paths use **pathlib** or **os.path** 
- for listing files of similar pattern use **glob.glob**

---

# Pickling and JSON in Python
## Pickle 
Pickling is used to serialize a python object to binary format. It usually consists of two steps **Serializing and Deserializing an Object**.

```python
import pickle

person = {"name":"Tanishq", "Age":22}

with open("person.pkl", "wb") as file:
	pickle.dump(person, file) """Serializing an object"""

with open("person.pkl", "rb") as file:
	person_retrieved = pickle.load(person, file) """Deserializing an object"""
```

## JSON 

```python
import json

person = {"name":"Tanishq", "Age":22}

with open("person.pkl", "w") as file:
	json.dump(person, file)

with open("person.pkl", "r") as file:
	person_retrieved = json.load(person, file) 
```

|**Feature**|**Pickle**|**JSON**|
|---|---|---|
|**Format**|Binary|Text|
|**Human-readable**|No|Yes|
|**Speed**|Faster|Slower|
|**Compatibility**|Python-specific|Language-independent|
|**Custom objects**|Yes, supports any Python object|Limited to basic types like dict, list, etc.|
|**Use cases**|Saving Python-specific data efficiently|Sharing data across languages|

---

# OOPs in Python
A **class** is a blueprint for creating objects. Objects are instances of classes, and each object can have attributes (variables) and methods (functions defined in the class).
### Creating a Basic Class in Python

Let's say we're creating a class for a simple concept: a `Dog`. Each `Dog` object we create could have attributes like `name` and `age`.

Here’s what a basic class might look like in Python:

```python
class Dog:
    pass
```

This code defines an empty class called `Dog`. The `pass` statement tells Python to do nothing – it's a placeholder, so we don't get an error.

### Adding the `__init__` Method

The `__init__` method is a special method in Python that is called automatically every time a new object is created. It’s often referred to as the "initializer" or "constructor" of the class. This method is where we set up the initial values of our object's attributes.

Let's update our `Dog` class to include some attributes: `name` and `age`.

```python
class Dog:
    def __init__(self, name, age):
        self.name = name
        self.age = age
```

Here’s what’s happening:

- `def __init__(self, name, age):` defines the `__init__` method.
- `self` represents the instance of the class (the specific `Dog` object we’re creating).
- `self.name = name` sets the `name` attribute for the object, and `self.age = age` sets the `age` attribute.

### Using  self
```python
dog1=Dog("Kutta", 10)
```

**self**  is a reference to the current instance of the class. It lets you access the instance’s attributes and methods from within the class.

When you call a method on an object, Python automatically passes the object as the first argument, which we call `self`. You don’t have to explicitly pass it; Python does it for you.

When we call any method of the class, **self** is converted into object/instance name for example **dog1.bark()** is actually **Dog.bark(dog1)** considering we have a method bark in class Dog. 

### Adding a Method to Our Class

Now, let’s add a simple method to make our dog "speak."

```python
class Dog:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def speak(self):
        print(f"{self.name} says woof!")
```

- `speak` is a method, just like a function, but it’s defined within a class.
- `self` is used as the first parameter, allowing the method to access the object's attributes.

### Creating an Object (Instance) of the Class

To create an object, we call the class like a function, passing in any required arguments. Let’s make a dog named “Buddy” who is 3 years old.

```python
my_dog = Dog("Buddy", 3)
```

Here’s what happens:

- `Dog("Buddy", 3)` calls the `__init__` method.
- Inside `__init__`, `self.name` is set to "Buddy", and `self.age` is set to 3.

Now `my_dog` is an instance of `Dog` with its own unique `name` and `age`.

### Using the Object’s Methods and Attributes

Once we have an object, we can access its attributes and call its methods.

```python
print(my_dog.name)  # Output: Buddy
print(my_dog.age)   # Output: 3
my_dog.speak()      # Output: Buddy says woof!
```

### Example of Another Class

Let’s create a class called `Book` with attributes for `title`, `author`, and `pages`. We’ll add a method to print out a summary of the book.

```python
class Book:
    def __init__(self, title, author, pages):
        self.title = title
        self.author = author
        self.pages = pages

    def summary(self):
        print(f"'{self.title}' by {self.author}, {self.pages} pages long.")
```

Now let’s create an instance of the `Book` class:

```python
my_book = Book("To Kill a Mockingbird", "Harper Lee", 281)
print(my_book.title)        # Output: To Kill a Mockingbird
print(my_book.author)       # Output: Harper Lee
my_book.summary()           # Output: 'To Kill a Mockingbird' by Harper Lee, 281 pages long.
```

---

