---
paths:
  - "**/*.py"
  - "**/*.pyi"
---
# Python Coding Style

> This file extends [common/coding-style.md](../common/coding-style.md) with Python specific content.

## Standards

- Follow **PEP 8** conventions
- Use **type annotations** on all function signatures

## Immutability

Prefer immutable data structures:

```python
from dataclasses import dataclass

@dataclass(frozen=True)
class User:
    name: str
    email: str

from typing import NamedTuple

class Point(NamedTuple):
    x: float
    y: float
```

## Formatting

- **black** for code formatting
- **isort** for import sorting
- **ruff** for linting

## Pythonic Idioms (CRITICAL)

### Dictionary Operations

```python
# GOOD: Use .get() with default
value = data.get('key', 'default')

# BAD: Manual check
value = data['key'] if 'key' in data else 'default'

# GOOD: Dictionary comprehension
squares = {x: x**2 for x in range(10)}

# GOOD: Merge dictionaries (3.9+)
merged = dict_a | dict_b
```

### Error Handling

```python
# GOOD: Raise early, explicit checks
def divide(a: float, b: float) -> float:
    if b == 0:
        raise ZeroDivisionError("除数不能为零")
    return a / b

# GOOD: match-case for multiple branches (3.10+)
def calculate(a: float, b: float, op: str) -> float:
    match op:
        case '+': return a + b
        case '-': return a - b
        case '*': return a * b
        case '/': return a / b
        case _: raise ValueError(f"未知运算符: {op}")

# BAD: Complex inline expressions in dict
operations = {
    '/': a / b if b != 0 else (_ for _ in ()).throw(ZeroDivisionError())
}
```

### Comprehensions

```python
# GOOD: List comprehension
names = [user.name for user in users if user.active]

# GOOD: Generator for large data (memory efficient)
names = (user.name for user in large_dataset)

# GOOD: Nested comprehension (when readable)
matrix = [[i*j for j in range(5)] for i in range(5)]

# BAD: Overly complex comprehension
# Use regular loops for readability when logic is complex
```

### Context Managers

```python
# GOOD: Always use context manager for resources
with open('file.txt') as f:
    content = f.read()

# GOOD: Custom context manager
from contextlib import contextmanager

@contextmanager
def timer():
    import time
    start = time.time()
    yield
    print(f"Elapsed: {time.time() - start}s")
```

### Anti-Patterns to Avoid

```python
# BAD: Using generator expression to throw exception
return (_ for _ in ()).throw(SomeError())

# GOOD: Just raise directly
raise SomeError()

# BAD: Checking type with type()
if type(obj) == list:

# GOOD: Use isinstance()
if isinstance(obj, list):

# BAD: Mutable default argument
def add_item(item, items=[]):

# GOOD: Use None as default
def add_item(item, items: list | None = None):
    if items is None:
        items = []
```

## Reference

See skill: `python-patterns` for comprehensive Python idioms and patterns.
