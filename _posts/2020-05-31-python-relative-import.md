---
title: How to Properly Use Relative Import
tags: python module
img_url: /assets/img/python.png
---

As I learn more about file control in Python, I
stumbled upon two options for import: absolute
import and relative import. 

Prior to that, there is a need of understanding 
how import system works in Python. 

When import occurs, Python finds the package, module
, resource, or whatever you attempt to retrieve
in the order of sys.modules(cache for modules),
built-in modules, and list of directories defined
by sys.path (current directory and etc.).

With this in mind, absolute import is done by showing
import path starting from any directory found from
the process above. Otherwise, relative path gives
freedom to import sources relative to the file
location. 

This is useful but there are caveats.

When I tried to achieve relative import, it caused
an error as such:
```bash
Traceback (most recent call last):
  File "test.py", line 1, in <module>
    from .word_vector import *
ImportError: attempted relative import with no known parent package
```
my file structure looked like this:
```bash
nlpyj
    -text
        -...
    -word_vector.py
    -test.py
```
and with in test.py
```python
# test.py
from .word_vector import *
...
```

The problem was that my test.py file ran
within the package and it prevented Python to realize
word_vector.py is a part of the package. word_vector.py
was found _way to early_ according to https://stackoverflow.com/questions/14132789/relative-imports-for-the-billionth-time.

The link explains nitty gritty of why Python throws
such error. I think the best to avoid is to understand
the caveat of relative import and don't use it unless
you really need to. 