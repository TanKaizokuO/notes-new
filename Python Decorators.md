---
tags:
  - main
---
---
## Part 1: Defining a Decorator

### What Is a Decorator?

In Python, a decorator is a function that modifies the behavior of another function or method. It allows for the addition of functionality to existing code in a clean and readable manner without altering the original function's structure.

### Why Use Decorators?

Decorators are useful for:

- **Code Reusability**: Encapsulate common functionality (e.g., logging, authentication).
- **Separation of Concerns**: Keep core logic separate from auxiliary tasks.
- **Enhanced Readability**: Apply functionality declaratively using the `@decorator_name` syntax.

---

## Part 2: Simple Examples

### Example 1: `@classmethod`

A `@classmethod` is a method bound to the class, not the instance. It receives the class (`cls`) as the first argument and can modify class state.

```python
class Employee:
    raise_amount = 1.05

    def __init__(self, name, salary):
        self.name = name
        self.salary = salary

    @classmethod
    def set_raise_amount(cls, amount):
        cls.raise_amount = amount

# Usage
Employee.set_raise_amount(1.10)
print(Employee.raise_amount)  # Output: 1.1

```

In this example, `set_raise_amount` modifies the class variable `raise_amount`, affecting all instances of `Employee`.

### Example 2: `@staticmethod`

A `@staticmethod` does not receive an implicit first argument (neither `self` nor `cls`). It's a method that doesn't access or modify class or instance state.

```python
class MathOperations:
    @staticmethod
    def add(x, y):
        return x + y

# Usage
result = MathOperations.add(5, 3)
print(result)  # Output: 8

```

Here, `add` is a utility function that logically belongs to the class but doesn't interact with class or instance data.

---

## Part 3: Advanced Example — `@function_tool` in OpenAI Agents SDK

In the OpenAI Agents SDK, the `@function_tool` decorator converts a regular Python function into a tool that an AI agent can use.

### Example: Creating a News Fetching Tool

```python
from agents import function_tool

@function_tool
def get_news_articles(topic: str) -> str:
    """Fetches news articles related to the given topic."""
    # Simulated implementation
    return f"News articles about {topic}"

```

By decorating `get_news_articles` with `@function_tool`, it becomes a callable tool for agents within the SDK. The decorator handles:

- Parsing the function signature to create a JSON schema for parameters.
- Using the docstring to describe the tool's purpose.
This integration allows agents to understand and utilize the function effectively.

---

## Part 4: Behind the Scenes — Creating Custom Decorators

### Understanding Decorators

A decorator is essentially a function that takes another function as an argument, adds some functionality, and returns another function.

### Example: Creating a Simple Logging Decorator

```python
import functools

def log_decorator(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        print(f"Calling function: {func.__name__}")
        result = func(*args, **kwargs)
        print(f"Function {func.__name__} completed")
        return result
    return wrapper

@log_decorator
def greet(name):
    print(f"Hello, {name}!")

# Usage
greet("Alice")

```

**OUTPUT : ** 
```
Calling function: greet
Hello, Alice!
Function greet completed

```
### How It Works

1. **`log_decorator`**: Accepts the original function `func` as an argument.
2. **`wrapper`**: Defines a new function that adds logging before and after calling `func`.
3. **`@functools.wraps(func)`**: Preserves the original function's metadata (e.g., name, docstring).
4. **`@log_decorator`**: Applies the decorator to `greet`, replacing it with `wrapper`.