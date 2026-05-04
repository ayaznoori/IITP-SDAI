# Pre-Read: Python Modules, Package Management & Virtual Environments

## In this pre-read, you'll discover:

- How **modules** let you organize code into separate files and reuse it easily
- What **pip** is and how to install external packages with one simple command
- Why **virtual environments** keep your projects isolated and conflict-free
- How **import statements** connect different parts of your Python code
- Ways to organize your own projects using **packages**

---

## Detailed Explanation

### What Is a Module?

A **module** is simply a Python file containing code you can use in another file. Think of it like a recipe card. You write your code once in one file, then import it wherever you need it.

**Why it matters:** Modules stop you from copying and pasting the same code everywhere. You fix a bug in one place, and it fixes everywhere.

```python
# In greeting.py
def say_hello():
    print("Hello!")

# In main.py
import greeting
greeting.say_hello()
```

### What Is a Package?

A **package** is a folder containing multiple modules, plus a special file called `__init__.py`. It's like a cookbook holding many recipe cards together.

```python
# Import from a package
from utilities import math_helpers, string_helpers
```

### Understanding Import Statements

**Import statements** bring code from one module into another. The **namespace** is just the collection of names (functions, variables) available after importing.

```python
import math            # Import the whole math module
from os import path    # Import just one item
import pandas as pd    # Import with a shorter nickname
```

### Python Standard Library

Python comes with built-in modules ready to use. This is called the **standard library**. You don't need to install anything.

- `os` — interact with your operating system
- `datetime` — work with dates and times
- `json` — read and write JSON data
- `random` — generate random numbers
- `math` — mathematical functions

```python
import datetime
print(datetime.datetime.now())
```

### Installing External Packages with pip

**pip** is Python's package installer. It downloads packages from the Python Package Index (PyPI). You type one command, and pip does the rest.

```bash
pip install requests
pip install numpy
```

After installing, you import them like standard library modules:

```python
import requests
response = requests.get("https://example.com")
```

### Virtual Environments

A **virtual environment** is an isolated space where your project lives. Each project can have its own packages and versions, separate from other projects.

**Why it matters:** Project A might need version 1.0 of a package, while Project B needs version 2.0. Without virtual environments, they'd conflict and break.

```bash
python -m venv myenv
source myenv/bin/activate    # On Mac/Linux
myenv\Scripts\activate       # On Windows
```

Once activated, any `pip install` goes only into that environment.

### Dependency Management

**Dependencies** are the packages your project needs to run. **Dependency management** means tracking and installing them consistently, often using a `requirements.txt` file.

```bash
pip freeze > requirements.txt    # Save all packages
pip install -r requirements.txt   # Install from file
```

---

## Practice Exercises

1. Create a file called `helpers.py` with a function that prints your name. Import and run it from another file.

2. Use the `random` module to print a random number between 1 and 10.

3. Install the `requests` package using pip, then import it and print the version.

4. Create a virtual environment, activate it, and verify it's active (your terminal should show the environment name).

5. Create a folder called `mypackage` with an `__init__.py` file inside. Add a module and import it.

6. Run `pip freeze` to see all packages installed in your current environment.