# GMT211 Packaging Tutorial

## 1. Project Structure

Your folder structure should look like this:

```
your_project/
├── your_project_code/
│   ├── your_project.py
│   └── __init__.py
├── tests/
│   └── test_your_project.py
├── setup.py
├── requirements.txt
├── README.md
└── .github/
    └── workflows/
        └── python-test.yml
```

- **your_project.py**: Your main implementation (e.g., shortest path algorithm).
- **test_your_project.py**: `pytest` file with constraints to validate the logic.
- **requirements.txt**: Required packages written line by line (e.g., `numpy`, `pytest`).
- **README.md**: Project description and link to Sphinx docs.
- **.github/workflows/python-test.yml**: GitHub Actions workflow file for CI.
- **setup.py** and **__init__.py**: Required for packaging and uploading to TestPyPI.

---

## 2. Generate Sphinx Documentation

### Install Required Tools

```bash
pip install sphinx sphinx_rtd_theme
```

### Create Initial Sphinx Files

Navigate to your project root directory and run:

```bash
sphinx-quickstart docs
```

Answer prompts as follows:

```
> Separate source and build directories (y/n) [n]: y
> Project name: YourProjectName
> Author name(s): YourName
> Project release: 0.1
```

### Auto-generate `.rst` Files

```bash
cd docs
sphinx-apidoc -o source ../projekodları
```

### Build HTML Documentation (on Windows)

```bash
make.bat html
```
or
```
.\make.bat html
```

Open the file at:

```
docs/build/html/index.html
```

### Regenerate Documentation After Updates

```bash
sphinx-apidoc -f -o docs/source ../projekodları
cd docs
.\make.bat html
```

---

## 3. Upload to GitHub

Initialize a Git repository and push to your GitHub repo:

```bash
git init
git remote add origin https://github.com/yourusername/yourrepo.git
git branch -M main
git push -u origin main
```

---

## 4. Prepare Files for TestPyPI

TestPyPI is a sandbox version of the Python Package Index.

###  Install Required Packages

```bash
pip install setuptools wheel twine
```

###  Example `setup.py`

```python
from setuptools import setup, find_packages

with open("README.md", "r") as f:
    description = f.read()

setup(
    name='shortestpath00000000',  # Replace with your student ID
    version='0.1.0',
    author='Your Name',
    author_email='youremail@example.com',
    description='A brief description',
    long_description=description,
    long_description_content_type='text/markdown',
    url='https://github.com/yourusername/yourrepo',
    packages=find_packages(),
    install_requires=[
        'numpy>=1.21.0',
        'pandas>=1.3.0',
    ],
    classifiers=[
        'Programming Language :: Python :: 3',
        'Operating System :: OS Independent',
    ],
    python_requires='>=3.6',
)
```

---

##  Build the Distributions

In your project directory:

```bash
python setup.py sdist bdist_wheel
```

Or (recommended):

```bash
pip install --upgrade build
python -m build
```

Check the `dist/` folder for `.tar.gz` and `.whl` files.

---

## 5. Upload to TestPyPI

1. Create an account: https://test.pypi.org/account/register/  
2. Generate an API token: https://test.pypi.org/manage/account/#api-tokens

Then upload your package:

```bash
python -m twine upload --repository testpypi dist/*
```

Paste the API token when prompted.

---

##  Install Your Test Package

To install from TestPyPI:

```bash
pip install -i https://test.pypi.org/simple/your_package_name
```

If uploaded to the real PyPI:

```bash
pip install shortestpath00000000
```
