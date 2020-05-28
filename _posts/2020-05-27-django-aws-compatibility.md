---
title: AWS Deployment, Check for Compatibility
tags: aws django python
---

I used Django 3.0.5 to develop my portfolio website originally.
Unfortunately, I overlooked the version compatibility of AWS with Django.
The specified version was 2.1.1 with Python 3.6 the latest.
This made me configure the project to 2.1.1, basically recreating the project and moving the file in to the project root.
I think one of the reasons why AWS is maintaining such compatibility is due to SQLite.
Once I did

```bash
eb logs
```
I found out that SQLite 3.8 which is a default db provided by Django is not provided by AWS
(it still maintained 3.7 version).
    
This made me reconsider the value of virtualenv sciprt.
```bash
brew install pyenv
pyenv install 3.6
virtualenv -p [LOCATION_OF_PYTHON_INSTALLED]/bin/python.3.6 [VIRTUAL_ENVIRONMENT]
source [VIRTUAL_ENVIRONMENT]/bin/activate
...

deactivate
```

This gets very handy when I have to face compatibility issue.



