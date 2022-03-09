# Django Project: Documentation

### This documentation provides detailed steps on creating a Django project, as well as important information to remember during your build. This documentation also goes over how to connect your Django project to a PostgreSQL database.

---

<br>

### **Terminal: Create Database**

Create the PostgreSQL database for your project:   

`createdb car-collector`   

<br>

### **Terminal: Head to where your projects are kept:**

Example of where I keep my projects in my computer
<br>

`cd ~/Desktop/`   
`cd SEIR/`   
`cd HW + Projects/`

<br>

### **Terminal: Run command to create Django project:**

This will create a directory with the title of "car-collector"
<br>

`django-admin startproject car-collector`

<br>

### **Terminal: Head to *django_env* directory:**

Running the first command below anywhere in your terminal will take you to your root directory
<br>

`cd ~`   
`cd django-env/`

<br>

### **Terminal: Enter command to run django environment:**

`pipenv shell`

<br>

### **Terminal: Head to your *car_collector* project:**

`cd ~/Desktop/`   
`cd SEIR/`   
`cd HW + Projects/`   
`cd car-collector/`

<br>

### **Terminal: Initialize a Git repository:**

`git init`   
`git remote add origin <name of repo link>`   
`git remote -v` \* checks if remote was added   
`git add -A`   
`git commit -m "initialized Django project"`   
`git push origin master`

<br>

### **Terminal: Open VSCode**

`code .`

<br>

### **Make sure that your Python Interpreter with django-environment is set up:**

> command + shift + P    
> Select Correct Interpreter

Interpreter should look something like this: <br>
`Python 3.9.10 ('django_env-NuObBWWW')`

\* May need to shut down terminal and re-start to see django environment pop up.

<br>

### **VSCode Terminal: Run command to start your Django app directory**

`django-admin startapp main_app`

 <br>

### **Install main_app to *car_collector*:**

1.) Head to **settings.py** under **car_collector**    
2.) Find `INSTALLED_APPS` list and add `main_app` to it

<br>

### **VSCode Terminal: Run server:**

`python manage.py runserver`

**\*We will see migrations warning, but should see successful start to the server.**

<br>

### **Head to *localhost:8000* in your browser**

You should see a success screen pop-up with a spaceship.

<br>

### **Head to *settings.py* under *car_collector*:**

1.) Find `DATABASES {}` dictionary   
2.) Change:   
`'NAME': BASE_DIR / 'db.sqlite3',` to   
`'NAME': 'car_collector',` \* database name/name of project

<br>

### **Transfer migrations to database:**

*Migrations:* ways to keep track of changes made to databases overtime --> especially changes made to models (will be covered later in documentation)

1.) Shut down terminal using **control + C**   
2.) Run command: `python manage.py migrate`

\* In beginning of project: the migrations represent all of the initial changes we need to make in order to create database (even though database is already created, tables need to be added to database)   
\* These tables are put in place to manage different parts of our project internally (manage various utilities behind the scenes)   
\* Migrate = transferring from file to database system

<br>

### **Connect core of project to Django app**

\* Core project = **car_collector** --> project's entry point (similar to server.js in Express but with multiple files)   
\* Django app = **main_app**--> controllers, models, anything specific to routing goes here

1.) In VSCode terminal, add file to **main_app** called **urls.py**:    
`touch main_app/urls.py`   
2.) Need to connect **urls.py** "urls config" in **main_app** to **urls.py** "urls config" in **car_collector**:

![Components of Django Project](https://i.imgur.com/gscrXbn.png)

- Head to **urls.py** in core project (**car_collector**); under `urlpatterns []` list, add:   

> `from django.urls import path, include`   
>    
> `urlpatterns = [`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `...`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `path('', include('main_app.urls')),`   
> `]`

\* `''` above: root url path (empty string)   
\* for the root path, include Django app (**main_app**) and **urls.py** / "urls config" file

<br>

### **Define URL patterns in *urls.py* inside *main_app*:**

> `from django.urls import path`   
> 
> `urlpatterns = [`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `path('', views.home, name='home'),`   
> `]`

\* All `urls.py` modules need this variable inside of them --> Django will scan file and look for variable and any url paths defined in that list variable   
\* *path:* method used to create url paths; generates and allows us to define url paths   
\* `''` above: Home page/url path   
\* `views.home` --> functionality we want to invoke in result of path being requested (home view function in **views.py**)   
\* `name='home'` --> alias for url pattern; instead of referencing path itself, we can reference name attribute of path whenever we are trying to create dynamic paths in our templates (will be shown later in documentation)

<br>

### **Head to *views.py* in *main_app* (controllers file):**

\* *view functions* are invoked in response to a request being made to server (heading to a specific path)

1.) Bring controller into scope:   
`from . import views`   

2.) Add home views function:
> `from django.http import HttpResponse`    
>
> `def home(request):`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`return HttpResponse("hello")`    

\* `return` --> response of request   
\* `request` --> similar to requires in Express, but just putting request instead    
\* Try refreshing the page; you should see "hello world" in browser window   
\* Write similar view functions for your about page and any other pages (define path as well in **urls.py** in **main_app**)

<br>

****

### **Django Templates**

**DTL/DJango Template Language** - domain-specific language to framework; renders dynamic HTML

1.) Add templates folder to **main_app**:   
`mkdir main_app/templates`    

2.) Add **about.html** to **templates** folder:   
`touch main_app/templates/about.html`

3.) Add HTML boilerplate:   
> ! + ENTER or TAB

- Change title of HTML document
- Add content to `<body>` tags

4.) Serve HTML templates:   
- Head to **views.py**/controllers file in **main_app**
- Change **about** views function

> `def about(request):`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `return render(request, 'about.html')`

\* If you head to **localhost:8000/about/**, you should see HTML rendered to DOM instead of string HttpResponse we had earlier   
\* Do the same for the home page (change **home** views function to render **home.html** template; create **home.html** template with boilerplate content)

<br>

### **Template Inheritance**

\* Can be compared to EJS Partials

1.) Create **base.html** inside of **templates** folder:   
`touch main_app/templates/base.html`

2.) Sharing **base.html**: extend **base.html** in other HTML templates   
- Any place we want to override content: use content blocks
- **base.html** --> blueprint for all templates; change blueprint for specific HTML templates using content blocks

`{% %}` --> Django template tags   
`{% block content %} {% endblock %}` --> placeholder/content block: extend **base.html** into sub-template (ex: **about.html**)   

\* We can override what is inside this block by defining it in sub-templates and add the content to put into place   
\* You will see this come into practice with future code blocks

3.) Create boilerplate inside of **base.html** and within the `<body>` tags, add `<main>` tags, and add within those tags:    

> `{% block content %} `  
>   
> `{% endblock %}`

4.) Inside of **about.html**, take out entire boilerplate and add this to extend **base.html** and replace block content with what you want to be shown in about page specifically:   

> `{% extends 'base.html' %}`   
>   
> `{% block content %}`   
> `<h1> About The Car Collector </h1>`   
> `</hr>`   
> `<p>Hire The Car Collector</p>`   
> `{% endblock %}`

\* When **base.html** is extended in sub-templates, it is basically carrying over the entire HTML boilerplate over to the sub-templates. Whatever is defined within a **content block** in a sub-template, will override the **content block** in the exended **base.html template** --> so all this content will be within `<main>` tags since the **content blocks** in **base.html** were defined within these tags.   
\* Extend **base.html** into your other sub-templates

****

### **Static folder: CSS + JS files**

\* Similar to Express' public folder

1.) Add **static folder** to **main_app**:    
`mkdir main_app/static`

2.) Add CSS folder to **static folder**, then add **style.css** file to CSS folder:    
`mkdir main_app/static/css`    
`touch main_app/static/css/style.css`

3.) Inside **base.html**, add to top of boilerplate (even above `<!DOCTYPE html>`):   
`{% load static %}`

\* This tells Django view engine that we need to make the static assets available in our templates

4.) Link to stylesheet in **base.html** boilerplate:   
`<link rel="stylesheet" href="{% static 'css/style.css' %}/>"`

\* May need to reset server to see any CSS changes   
\* `{% static 'css/style.css' %}` --> static template tag with expected dynamic path to static assets

<br>

### **Rendering data to Templates**

1.) Add nav bar to **base.html** (inside additional `<header>` tags):   

> `<header>`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<ul>`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<li><a href="/about">About</a></li>`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<li><a href="/cars">Cars</a></li>`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`</ul>`   
> `</header>`

<br>

2.) Add cars path to **urlpatterns** list in **main_app**:   

`path('cars/', views.cars_index, name='index'),`   

<br>

3.) Define **cars_index** views function in **views.py** in **main_app**:   

> `def cars_index(request):`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `return render(request, 'cars/index.html', { 'cars': cars })`

\* `cars/index.html` --> folder with template where we are setting data to   
\* `{'cars': cars}` --> context dictionary (similar to context object we utilized in Express/node.js); we use this to pass data from our view function to the rendered template; currently will have green squiggly line below it because **cars** is not defined yet

4.) Create fake database (will change later when we use PostgreSQL for database --> this step is for testing the render of data to a template):

> `class Car():`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`def __init__(self, make, model, color, description):`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`self.make = make`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`self.model = model`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`self.color = color`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`self.description = description`
>    

> `cars = [`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`car('Tesla', 'Model X', 'blue', 'my dream car I will name Blueberry'),`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`car('Kia', 'EV6', 'silver', 'saw during the superbowl commercial and... wow'),`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`car('Kia', 'Stinger', 'Ascot Green', 'fast car vrooom')`   
> `]`

5.) Create template rendering all cars:   

`mkdir main_app/templates/cars`   
`touch main_app/templates/cars/index.html`

6.) Head to **cars/index.html**, extend **base.html** and add content for rendering all cars from **cars** list:   

> `{% extends 'base.html' %}`   
>    
> `{% block content %}`   
> `<h1>Car List</h1>`   
>    
> `{% for car in cars %}`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<div>`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<span>{{ car.make }}</span>`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<span>{{ car.model }}</span>`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<span>Color: {{ car.color }}</span>`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<span>Description: {{ car.description }}</span>`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`</div>`   
> `{% endfor %}`   
> `{% endblock %}`

\* `{% endfor %}` --> ending for loop block tag (if it was an if statement, the ending block tag would be: `{% endif %}`)

****

### **Django Models**

\* *model:* extension of Django's built-in **Model class**   
\* Attributes in **Model class** are created with **Field classes**; each attribute will become a *column in the table* for that specific model's table.

