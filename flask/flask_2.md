---
layout: default
title: Flask, Part II
nav_order: 2
---

You have many choices on how to deploy your application to the web.  You can rent a virtual machine from AWS, Google Cloud or Digital Ocean.  

Haverford and Bryn Mawr students can use [sites.haverford](sites.haverford.edu) or [digital.brynmawr](digital.brynmawr.edu).
Here are instruction to deploy a Flask app there: [http://bit.ly/flask-deploy](http://bit.ly/flask-deploy)

Heroku is a popular tool for deploying Python applications.  [here is a tutorial on how to deploy your app using Heroku](https://medium.com/the-andela-way/deploying-a-python-flask-app-to-heroku-41250bda27d0)

If your app is primarily used for content publishing or if the appication's processing takes place in the browser, you can easily "freeze" your app.  Frozen-Flask creates a static version of your app and all its contents. You can then deploy them GitHub pages or Netify. This is free and there are other advantages to "minimal computing," which Alex Gil outlines [here](https://des4div.library.northeastern.edu/design-for-diversity-the-case-of-ed-alex-gil/).  In industry, this approach to deployment is often called a [JAMstack](https://jamstack.org/)

[Frozen-Flask](https://pythonhosted.org/Frozen-Flask/)
`$ pip install Frozen-Flask`

**freeze.py**
```python
import csv
from flask_frozen import Freezer
from app import app

freezer = Freezer(app)

with open('courses.csv','r') as f:
    reader = csv.DictReader(f) 
    info = ""
    departments = []
    for row in reader:
        departments.append(row['department'])

@freezer.register_generator
def by_subject():
    for department in departments:
        if department == 'Latin American, Iberian and Latina/o Studies':
            pass
        else:
            yield {'department': department}


if __name__ == '__main__':
    freezer.freeze()
```

[source](https://medium.com/@francescaguiducci/how-to-build-a-simple-personal-website-with-python-flask-and-netlify-d800c97c283d)
