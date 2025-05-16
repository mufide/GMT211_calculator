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

- **your_project.py**: Edit file name. Your main implementation (e.g., shortest path algorithm).
- **test_your_project.py**: `pytest` file with constraints to validate the logic.
- **requirements.txt**: Required packages written line by line (e.g., `numpy`, `pytest`).
- **README.md**: Project description and link to Sphinx docs.
- **.github/workflows/python-test.yml**: GitHub Actions workflow file for CI.
- **setup.py** and **__init__.py**: Required for packaging and uploading to TestPyPI. It will be mentioned in the relevant step.

---

## 2. Generate Sphinx Documentation

Sphinx documentation is a detailed user guide for project codes. Readme or Project description on testpypi is a general explanation of what the code does and for what purposes it can be used.

### Install Required Tools

```bash
pip install sphinx sphinx_rtd_theme
```

### Create Initial Sphinx Files

Navigate to your project root directory and run:
(You can also use another theme)
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
After this process, files such as source/, build/, Makefile, make.bat are created under the docs folder.

### Auto-generate `.rst` Files

```bash
cd docs
sphinx-apidoc -o source ../your_project_code
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
You can double-click the index.html file and open it in your browser.

### Regenerate Documentation After Updates

```bash
sphinx-apidoc -f -o docs/source ../your_project_code
cd docs
.\make.bat html
```

---

## 3. Upload to GitHub

Initialize a Git repository and push to your GitHub repo (https://github.com/mufide/GMT211-Tutorial).
If you have trouble uploading to the main branch, follow these steps;
```bash
git init
git remote add origin https://github.com/yourusername/yourrepo.git
git branch -M main
git push -u origin main
```

---

## 4. Prepare Files for TestPyPI

TestPyPI is a sandbox version of the Python Package Index. We will upload homework or trial packages here to avoid creating unnecessary weight by uploading the same packages to the PyPI database over and over again. However, you can use PyPI when you have completed other package tests, there are no permission restrictions.

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
    name='shortestpath00000000',  # Replace with your student ID !!
    version='0.1.0',
    author='Your Name',
    author_email='youremail@example.com',
    description='A brief description',
    long_description=description,
    long_description_content_type='text/markdown',
    url='https://github.com/yourusername/yourrepo',
    packages=find_packages(),
    install_requires=[ #You should write which packages are required in your project.
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

# Congratulations !
