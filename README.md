# **Library Development and Deployment**

## **Table of Contents**
1. **Introduction**
   - 1.1. What is a Python Library?
   - 1.2. Why Create Your Own Library?
2. **Setting Up the Development Environment**
   - 2.1. Virtual Environment Setup
   - 2.2. Common Errors and Fixes
3. **Creating Your Library**
   - 3.1. Structure of a Python Library
   - 3.2. Writing the Code (e.g., Calculator Library)
   - 3.3. Writing Tests for Your Library
4. **Packaging Your Library**
   - 4.1. The `setup.py` File
   - 4.2. Handling `setup.py` Errors
5. **Deploying the Library to PyPI**
   - 5.1. Installing Twine
   - 5.2. Preparing for Deployment
   - 5.3. Uploading to PyPI
   - 5.4. Troubleshooting Deployment Errors
6. **Installing Your Library**
   - 6.1. How to Install the Library
   - 6.2. Verifying Installation
7. **Maintaining Your Library**
   - 7.1. Updating Your Library Version
   - 7.2. Adding New Features
   - 7.3. Fixing Bugs and Releasing Updates
---

## **1. Introduction**

### **1.1 What is a Python Library?**
A Python library is a collection of Python code that provides reusable functionality to other Python programs. Libraries can include functions, classes, modules, and packages. Python libraries save time and effort, as they allow you to use pre-written, tested code instead of writing everything from scratch.

### **1.2 Why Create Your Own Library?**
Creating your own Python library:
- **Simplifies Reusability:** You can reuse the same code in multiple projects.
- **Easy Maintenance:** You can modify or enhance your library without having to rewrite code.
- **Share with the Community:** You can share your library on PyPI so others can install and use it.

---

## **2. Setting Up the Development Environment**

### **2.1 Virtual Environment Setup**
Using a **virtual environment** ensures that the dependencies of your project are isolated and do not interfere with your global Python installation.

1. **Install `virtualenv`** (if not already installed):
   ```bash
   pip install virtualenv
   ```

2. **Create a new project folder** and navigate into it:
   ```bash
   mkdir my_library
   cd my_library
   ```

3. **Create a virtual environment:**
   ```bash
   virtualenv venv
   ```

4. **Activate the virtual environment:**
   - On Windows:
     ```bash
     venv\Scripts\activate
     ```
   - On Mac/Linux:
     ```bash
     source venv/bin/activate
     ```

   The prompt should change, indicating that the virtual environment is active.

#### **Common Errors and Fixes:**
- **Error:** `virtualenv command not found`
  - **Fix:** Ensure `virtualenv` is installed. Run `pip install virtualenv`.
- **Error:** `Permission denied` when activating.
  - **Fix:** On Windows, make sure your terminal has administrator privileges.

---

## **3. Creating Your Library**

### **3.1 Structure of a Python Library**
A well-structured Python library typically has the following format:

```plaintext
my_library/
│
├── my_library/                # Library code (your Python package)
│   ├── __init__.py            # Marks the directory as a package
│   ├── module1.py             # A module containing functions or classes
│
├── tests/                     # Tests directory
│   ├── __init__.py
│   └── test_module1.py        # Tests for your library
│
├── setup.py                   # Setup script for packaging
├── README.md                  # Documentation file
└── LICENSE                    # License file
```

### **3.2 Writing the Code (e.g., Calculator Library)**

1. **Create your library folder (`my_library/`).**

2. **Write the functions for your calculator** (e.g., `module1.py`):
   ```python
   # my_library/module1.py

   def add(a, b):
       """Add two numbers."""
       return a + b

   def subtract(a, b):
       """Subtract second number from first."""
       return a - b

   def multiply(a, b):
       """Multiply two numbers."""
       return a * b

   def divide(a, b):
       """Divide first number by second. Raises error if dividing by zero."""
       if b == 0:
           raise ValueError("Cannot divide by zero")
       return a / b
   ```

3. **Write the `__init__.py` file** to make the `my_library` folder a package:
   ```python
   # my_library/__init__.py
   from .module1 import add, subtract, multiply, divide
   ```

### **3.3 Writing Tests for Your Library**
Testing ensures that your library functions as expected.

1. **Create a `tests/` directory** for your tests.

2. **Write unit tests for the library** in `test_module1.py`:
   ```python
   # tests/test_module1.py

   import unittest
   from my_library import add, subtract, multiply, divide

   class TestCalculatorFunctions(unittest.TestCase):
       
       def test_add(self):
           self.assertEqual(add(2, 3), 5)
       
       def test_subtract(self):
           self.assertEqual(subtract(5, 3), 2)
       
       def test_multiply(self):
           self.assertEqual(multiply(2, 3), 6)
       
       def test_divide(self):
           self.assertEqual(divide(6, 2), 3)
           with self.assertRaises(ValueError):
               divide(6, 0)

   if __name__ == "__main__":
       unittest.main()
   ```

---

## **4. Packaging Your Library**

### **4.1 The `setup.py` File**
The `setup.py` file is essential for making your library installable and deployable. Here's a basic `setup.py` for your calculator library:

```python
# setup.py

from setuptools import setup, find_packages

setup(
    name='my_calculator_library',       # Name of your library
    version='0.1.0',                    # Version of your library
    packages=find_packages(),           # Finds all packages and sub-packages
    install_requires=[],                # External dependencies (leave empty for now)
    test_suite='tests',                 # Test suite location
    author='Muhammad Raheel',                 # Your name
    author_email='mraheel.naseem@gmail.com', # Your email
    description='A simple calculator library', # Short description
    long_description=open('README.md').read(), # Detailed description
    long_description_content_type='text/markdown', # Format of the README
    url='https://github.com/muhammadraheelnaseem', # Repository URL
    classifiers=[                       # Python version and license information
        'Programming Language :: Python :: 3',
        'License :: OSI Approved :: MIT License',
    ],
)

```

### **4.2 Handling `setup.py` Errors**
- **Error:** `error: package directory 'my_library' does not exist`
  - **Fix:** Ensure the directory `my_library/` exists in the project folder and contains the Python code.

---

## **5. Deploying the Library to PyPI**

### **5.1 Installing Twine**
Twine is a utility for publishing Python packages to PyPI and other repositories.
`Twine` is used to securely upload your package to PyPI.

```bash
pip install twine
```

### **5.2 Preparing for Deployment**
1. **Build the distribution:**
   ```bash
   python setup.py sdist bdist_wheel
   ```

2. **Check the `dist/` folder** to ensure the `.tar.gz` and `.whl` files were generated.

### **5.3 Uploading to PyPI**
Run the following command and enter your API token when prompted:

```bash
twine upload dist/*
```

### **5.4 Troubleshooting Deployment Errors**
- **Error:** `403 Forbidden - User is not allowed to upload to project`
  - **Fix:** Ensure the project name is unique on PyPI. If the name is taken, rename your project in `setup.py`.
- **Error:** `invalid command 'bdist_wheel'`
  - **Fix:** Install `wheel` by running `pip install wheel`.

---

## **6. Installing Your Library**

### **6.1 How to Install the Library**
Once your library is uploaded to PyPI, users can install it with `pip`:

```bash
pip install my_calculator_library
```

### **6.2 Verifying Installation**
After installation, you can verify it by importing your library and using its functions:

```python
from my_calculator_library import add, subtract, multiply, divide
```

---

## **7. Maintaining Your Library**

### **7.1 Updating Your Library Version**
When making changes, increment the version number in `setup.py` according to [semantic versioning](https://semver.org/).

For example:
- `0.1.0` → `0.2.0` (new features)
- `0.2.0` → `0.2.1` (bug fixes)

### **7.2 Adding New Features**
Add new functions or modules to your library and update the `__init__.py` to include them.

### **7.3 Fixing Bugs and Releasing Updates**
If you fix any bugs, update the version and re-upload the new version to PyPI using the same steps.

