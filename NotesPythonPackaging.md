# Notes on *Publishing (Perfect) Python Packages on PyPi*
These notes are on a recording of a [talk at Europython Basel 2019](https://www.youtube.com/watch?v=GIF3LaRqgXo).  
**Note**: this is a legacy configuration using  *setup.py*. I still consider it useful to understand the process steps involved.


## 1 Creating a basic structure and *setup.py*
The barebone folder structure for the package looks something like this (although there's just one module in there yet):
```
setup.py
src/
|-- helloworld.py
```  
()*helloworld.py* contains only a singe function that prints "hello, world!".)

*setup.py* contains this code as minimum:  
```PYTHON
from setuptools import setup

setup(
    name="helloworld",
    version="0.0.1",
    description="Say hello!",
    py_modules=["helloworld"],
    packages_dir={"": "src"},
)
```


## 2 Testing the Build
in the terminal, run:
```BASH
$ python setup.py bdist_wheel
running bdist_wheel
...
copying src/helloworld.py -> build/lib
...
creating '/path/to/helloworld/dist/helloworld-0.0.1-py3-none-any.whl' and adding '.' to it
removing build/bdist.macosx-10.11-x86_64/wheel
```

The directory structure now looks like this:
```
build/
|-- bdist.macosx-10.11-x86_64/
|-- lib/
      |-- helloworld.py
dist/
|-- helloworld-0.0.1-py3-none-any.whl   # This is the final distribution
setup.py
src/
|-- helloworld.egg-info/    # This dir should be ignored
      |-- dependency_links.txt
      |-- PKG-INFO
      |-- SOURCES.txt
      |-- top_level.txt
|-- helloworld.py
```  

## 3 Installing it locally
For testing purposes, the package can be installed locally.
```BASH
# The -e links to the code instead of copying it to site-packages dir of the python distribution
$ pip install -e .
Obtaining file:///path/to/helloworld
Installing collected packages: helloworld
  Running setup.py develop for helloworld
Successfully installed helloworld
```

## 4 Perfecting it
- Adding a .gitignore, e.g. from [gitignore.io](https://gitignore.io)
- Add a licence, e.g. via [choosealicense.com](https://choosealicense.com)
- Add classifiers to setup.py (see [https://pypi.org/classifiers](https://pypi.org/classifiers))
- Add documentation (either ReStructuredText (use Sphinx) or **Markdown (use [MkDocs](https://www.mkdocs.org/))**)

TL;DR (or didn't type): Look at the other cool stuff that's in *setup.py*.  
Also, there are some other things to do for doing `python setup.py sdict`, especially creating a *MANIFEST.in* file. This is easily done with the *check-manifest* package:
```BASH
$ pip install check-manifest

$ check-manifest --create

$ git add MANIFEST.in
```

## 5 Pusblishing the package
Have a look at the recording starting from 00:24:00.

```BASH
# stick this in 'extras_require'
$ pip install twine

$ twine upload dist/*
```
