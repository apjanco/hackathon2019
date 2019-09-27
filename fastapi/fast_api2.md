```python
import csv
import random
from fastapi import FastAPI
from starlette.staticfiles import StaticFiles
from starlette.templating import Jinja2Templates
from starlette.requests import Request


app = FastAPI()
templates = Jinja2Templates(directory="templates")
app.mount("/static", StaticFiles(directory="static"), name="static")


@app.get("/")
async def read_root(request: Request):
    with open("courses.csv", "r") as f:
        reader = csv.DictReader(f)
        random_course = random.choice(list(reader))
        info = ""
        for i in random_course:
            info += (
                "<tr>"
                + "<th>"
                + i
                + "</th>"
                + "<td>"
                + random_course[i]
                + "</td>"
                + "</tr>"
            )
        # return render_template('index.html', course_info=info)
        return templates.TemplateResponse(
            "index.html", {"request": request, "course_info": info}
        )


@app.get("/{department}")
async def read_item(request: Request, department: str):
    with open("courses.csv", "r") as f:
        reader = csv.DictReader(f)
        info = ""
        for row in reader:
            if department in row["department"]:
                for i in row:
                    info += (
                        "<tr>"
                        + "<th>"
                        + i
                        + "</th>"
                        + "<td>"
                        + row[i]
                        + "</td>"
                        + "</tr>"
                    )
        # return render_template('index.html', course_info=info)
        return templates.TemplateResponse(
            "index.html", {"request": request, "course_info": info}
        )
 ```
