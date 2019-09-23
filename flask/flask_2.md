---
layout: default
title: Flask, Part II
nav_order: 2
---

You have many choices on how to deploy your application to the web.  You can rent a virtual machine from AWS, Google Cloud or Digital Ocean.  

Haverford and Bryn Mawr students can use [sites.haverford](sites.haverford.edu) or [digital.brynmawr](digital.brynmawr.edu).
Here are instruction to deploy a Flask app there: [http://bit.ly/flask-deploy](http://bit.ly/flask-deploy)

My recommended way to deploy your app is to "freeze" it.  We're going to create a static version of your app and all its contents and then deploy them using GitHub pages. This is free and there are other advantages to "minimal computing," which Alex Gil outlines [here](https://des4div.library.northeastern.edu/design-for-diversity-the-case-of-ed-alex-gil/).  In industry, this approach to deployment is often called a [JAMstack](https://jamstack.org/)


```python
from flask_frozen import Freezer
from myapp import app

freezer = Freezer(app)

if __name__ == '__main__':
    freezer.freeze()
```

[source](https://medium.com/@francescaguiducci/how-to-build-a-simple-personal-website-with-python-flask-and-netlify-d800c97c283d)
