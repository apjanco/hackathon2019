---
layout: default
title: Flask, Part I
parent: Monday
nav_order: 1
---

## Flask 
Flask is one of the simplest Python web frameworks available.
In the next few steps, we're going to: 
- Create a new folder called `flask_app`
- Create app.py in the `flask_app` directory
- Download [courses.csv](https://github.com/HCDigitalScholarship/summer-django/raw/master/courses.csv) to the `flask_app` directory
- create a templates subfolder  for our index.html file
- create a static subfolder for our main.css file

```
flask_app
    ├── app.py  
    ├── courses.csv
    ├── static    # Home to all of your images, javascript and CSS
    │   └── main.css
    └── templates  # Home for all of your HTML templates
        └── index.html
```

**[app.py](https://raw.githubusercontent.com/HCDigitalScholarship/summer-django/master/flask_app/app_v1.py)**
```python
import csv
import random
from flask import Flask, render_template


app = Flask(__name__)
application = app 


@app.route("/")   #This is the root URL. We can also create an absolute path @app.route("/about") or a relative one @app.route("/<user>")   
def index():
    with open("courses.csv", "r") as f:
        reader = csv.DictReader(f)
        random_course = random.choice(list(reader))
        info = ""
        for i in random_course:
            info += i + ":  " + random_course[i] + "<br>"
        return info


if __name__ == "__main__":
    app.run()
```

In the terminal run `$ python app.py`
> You'll get an error if you haven't installed Flask.  Just use `pip install flask`.  In the future we'll use virtual enviornments. If you'd like to get started now, you can install [virtualenv](https://virtualenv.pypa.io/en/latest/) or [Anaconda](https://www.anaconda.com/).  Real Python has a great tutorial on virtual enviornments [here](https://realpython.com/python-virtual-environments-a-primer/).    

We can now serve content from our csv file to the web.  Without any styling it's really ugly.  Let's add some HTML and CSS to make this look better.   

We'll need to make a few small changes to the application so that we're rendering an HTML template and not just sending text to the browser. Change this
```python
info = ""
for i in random_course:
    info += i + ":  " + random_course[i] + "<br>"
return info
```
to this
```python
info = ""
for i in random_course:
    info += "<tr>" + "<th>"+ i +"</th>" + "<td>" + random_course[i] + "</td>" + "</tr>"
return render_template('index.html', course_info=info)
```
> Here we're adding HTML tr and th tags which will create a table.  For more on HTML tables, see this [w3schools tutorial](https://www.w3schools.com/tags/tag_tr.asp) 

Create an index.html file in the `templates` directory (`mkdir templates`).  

**./templates/index.html**
```
<!DOCTYPE>
<html>

<head>
    <link rel="stylesheet" href="{{ url_for('static', filename='main.css') }}" 
</head>

    <body>

        <table style="width:100%">

            {{ course_info|safe }}

        </table>

    </body>

</html>

```

Now create `static/main.css`.  

**./static/main.css**
```css
@import url('https://fonts.googleapis.com/css?family=Cinzel');

table {
  border-collapse: collapse;
  width: 80%;
}

td, th {
  font-family: 'Cinzel', serif;
  border: 1px solid #4C061D;
  text-align: left;
  padding: 12px;
}

tr {
  background-color: #ffffff;
}

```
> Note the `@import url()`. This comes from [Google Fonts](https://fonts.google.com/).  You can use any of their thousands of multi-lingual fonts just by adding this import to your css.  For example, `@import url('https://fonts.googleapis.com/css?family=Roboto');` and then `font-family: 'Roboto', sans-serif;`


*your page should look something like this*  
![](https://github.com/HCDigitalScholarship/summer-django/raw/master/flask_demo.png) 

The cycle of life is complete.  You've taken data from the web and can now serve it back to your browser. More importantly, you can transform that information using Python.  You can sort classes by timeslot and create a schedule builder that integrates with [Google Calendar](https://developers.google.com/calendar/quickstart/python). It's all up to you. In the next section, we discuss how to deploy your application using sites.haverford.edu. 

[continue...](https://hcdigitalscholarship.github.io/summer-django/monday/flask_3.html)

### Bonus

One of the most useful features of dynamic web frameworks is the option to sort and organize data.  What if we wanted just courses in Physics?  You can add a new path that will take the subject from the URL in the browser and use that to filter your data. Just add the subject to your route and pass it as an argument to your view.  Django does this very elegantly, but I wanted to be sure you know that this is possible in Flask. 

```python
@app.route('/<department>')
def by_subject(department):
    with open('courses.csv','r') as f:
        reader = csv.DictReader(f) 
        info = ""
        for row in reader:
            if department in row['department']:
                for i in row:
                    info += "<tr>" + "<th>"+ i +"</th>" + "<td>" + row[i] + "</td>" + "</tr>"
        return render_template('index.html', course_info=info)
```
Click here to download the complete [app.py](https://raw.githubusercontent.com/HCDigitalScholarship/summer-django/master/flask_app/app.py)
