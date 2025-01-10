
# Python Code Blocks and Indentation

In Python, **code blocks** are defined by **indentation** rather than by braces `{}` or keywords (like `begin` and `end`). This indentation rule is essential for Python syntax and structure.


## Python Code Block Types

| **Code Block**          | **Definition**                                                                                              | **Syntax Example**                                          | **Explanation**                                                                                                   |
|-------------------------|------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| **Function Block**       | A block of code that defines a function.                                                                   | `def greet(name):`                                          | Functions start with `def`, followed by an indented block.                                                        |
|                         |                                                                                                            | `    print(f"Hello, {name}")`                               |                                                                                                                   |
| **Class Block**          | A block of code that defines a class.                                                                      | `class MyClass:`                                            | Classes start with `class`, followed by an indented block.                                                        |
|                         |                                                                                                            | `    def method(self):`                                     |                                                                                                                   |
|                         |                                                                                                            | `        pass`                                              |                                                                                                                   |
| **If-Else Block**        | A block of code that executes conditionally based on a condition.                                         | `if x > 0:`                                                 | Uses `if`, `elif`, and `else` keywords, each followed by a colon and an indented block.                           |
|                         |                                                                                                            | `    print("Positive")`                                     |                                                                                                                   |
|                         |                                                                                                            | `elif x == 0:`                                              |                                                                                                                   |
|                         |                                                                                                            | `    print("Zero")`                                         |                                                                                                                   |
|                         |                                                                                                            | `else:`                                                     |                                                                                                                   |
|                         |                                                                                                            | `    print("Negative")`                                     |                                                                                                                   |
| **For Loop Block**       | A block of code that iterates over a sequence.                                                            | `for i in range(5):`                                        | The `for` statement is followed by an indented block.                                                             |
|                         |                                                                                                            | `    print(i)`                                              |                                                                                                                   |
| **While Loop Block**     | A block of code that repeats as long as a condition is true.                                              | `while x > 0:`                                              | The `while` statement is followed by an indented block.                                                           |
|                         |                                                                                                            | `    x -= 1`                                                |                                                                                                                   |
| **Try-Except Block**     | A block of code that handles exceptions (runtime errors).                                                 | `try:`                                                      | Begins with `try`, followed by one or more `except` blocks.                                                       |
|                         |                                                                                                            | `    result = 1 / x`                                        |                                                                                                                   |
|                         |                                                                                                            | `except ZeroDivisionError:`                                 |                                                                                                                   |
|                         |                                                                                                            | `    print("Cannot divide by zero")`                        |                                                                                                                   |
| **With Block**           | A block of code that simplifies resource management (e.g., opening files).                                | `with open("file.txt") as f:`                               | The `with` statement is followed by an indented block.                                                            |
|                         |                                                                                                            | `    content = f.read()`                                    |                                                                                                                   |
| **List Comprehension**   | A concise block for creating lists in a single line.                                                      | `squares = [x**2 for x in range(10) if x % 2 == 0]`         | List comprehensions are single-line blocks enclosed in brackets.                                                  |
| **Dictionary Comprehension** | A concise block for creating dictionaries in a single line.                                                | `squares_dict = {x: x**2 for x in range(5)}`                | Dictionary comprehensions are single-line blocks enclosed in braces `{}`.                                         |
| **Docstring Block**      | A block of code that provides documentation for a module, class, or function.                             | `def greet():`                                              | Docstrings are enclosed in triple quotes `"""`.                                                                |
|                         |                                                                                                            | `    """This function greets the user."""`            |                                                                                                                   |
|                         |                                                                                                            | `    print("Hello")`                                        |                                                                                                                   |


## Indentation Rules

1. **Consistency**: Indentation must be consistent within a block. Python allows either spaces or tabs for indentation, but not both in the same code. PEP 8 recommends using **4 spaces per indentation level**.
   ```python
   if x > 0:
       print("Positive")  # 4 spaces of indentation
   ```

2. **Nested Blocks**: Each nested block must be indented further than the previous level.
   ```python
   def greet(name):
       if name:
           print(f"Hello, {name}")  # Nested block with 4 more spaces
   ```

3. **Colon (`:`)**: A colon is used to start a block after statements like `if`, `for`, `while`, `def`, `class`, and `try`.
   ```python
   if x > 0:  # Colon indicates the start of a block
       print("Positive")
   ```

4. **Empty Blocks**: Use the `pass` statement for empty blocks where syntax requires a statement but nothing needs to be done.
   ```python
   def placeholder_function():
       pass  # Placeholder for future code
   ```

## Common Indentation Errors

| **Error**                      | **Cause**                                                                 | **Example**                                         | **Solution**                                                                                                        |
|-------------------------------|---------------------------------------------------------------------------|----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------|
| **IndentationError**            | Mixing tabs and spaces for indentation.                                   | `if x > 0:`                                        | Use only **spaces** (recommended) or only **tabs** consistently.                                                   |
|                               |                                                                           | `    print("Positive")`                            |                                                                                                                    |
| **Unexpected Indent**           | Indenting without a preceding colon or control statement.                 | `    print("Positive")`                            | Ensure the block starts with a valid control statement or function definition.                                     |
| **IndentationError: Unindent** | Incorrect reduction of indentation level.                                 | `if x > 0:`                                        | Ensure the indentation level matches the structure of nested blocks.                                               |
|                               |                                                                           | `    print("End")`                                 |                                                                                                                    |
