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
\* `{{ }}` --> output of data into template

****

### **Django Models**

\* *model:* extension of Django's built-in **Model class**   
\* Attributes in **Model class** are created with **Field classes**; each attribute will become a *column in the table* for that specific model's table.

1.) Create model:

1.) Remove "fake" database from **views.py**   
2.) Head to **models.py** to initialize first model and define it's attributes:   

> `class Car(models.Model):`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`make = models.CharField(max_length=100)`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`model = models.CharField(max_length=100)`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`color = models.CharField(max_length=30)`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`description = models.TextField(max_length=250)`   

\* *models*: used to perform CRUD data operations on a database   
\* a Django model represents a single entity from the ERD   
\* **model objects:** represent rows in database table; also called instances of the Model, just like how we worked with Mongoose documents   
\* each attribute defined above wil be a column in the Car table   
\* the **Field class** in each attribute defines the column's data type in the table

2.) **Migration files**

`python manage.py makemigrations`

\* Above command: creates migration files for all models that have been added or changed since last migration   
\* Output from `makemigrations`:   
`main_app/migrations/0001_initial.py`   
\* This does NOT create a table in the database/update database schema   
\* Migrations directory is created for an app the first time you run `makemigrations`   
\* To create table in database, and to synchronize database with code in migration files, run this command next:   

`python manage.py migrate`   

To check that Car database was created, open psql shell:

`psql`   
`\l`   
`\c car_collector`   
`\d`

\* You should see `main_app_car` in database table

****

<br>

### **ORM: Object-Relational Mapper**

\* Creates Python objects from rows in a relational database table, and vice-versa   
\* allows developers to write object-oriented  code to CRUD data instead of using SQL directly   
\* Django's ORM automatically generates methods for each model:   
- filtering
- ordering
- can access data from related models

1.) Use Python shell to load django environment (performing CRUD):   

`python manage.py shell`

2.) Import model to work with, just like in the application:   

`from main_app.models import Car`

3.) Retrieve all cars (car objects)

`Car.objects.all()`

Response: `<QuerySet[]>`

\* To view all methods you can use on model objects:   
`Car.objects.` + `TAB` key twice   
\* Anytime you want to perform query operations on a Model to retrieve model objects (rows) from a database table, it is done via a **manager object**   
\* Django adds manager to every model class --> this is the **object's attribute attached to car above**   
\* `<QuerySet[]>` --> database query --> can be refined by adding additional methods to it   
\* When app needs data to iterate over cars, the query will be sent to the database and the result is a list-like object that represents a collection of model instances (rows) from the database

4.) Create in-memory model (instance of Car model) and save it to the database:

`c = Car(make="Tesla", model="Model X", color="blue", description="my future car will be named Blueberry")`

`c.__dict__`   

\* This will yield a response like: `{..., 'id': None, 'name': 'Tesla', etc...}`   

\* There is no id yet because it is not yet saved in the database. To save model instance to the database:   

`c.save()`   
`c.id` --> should yield 1

\* Now, when you check the database for all the car objects, you should get this:

`Car.objects.all()`   
`<QuerySet[<Car: Carobject(1)>]>`

\* You also save the model instance like this, in one line of code:   

`c = Car(make="Tesla", model="Model X", color="blue", description="my future car will be named Blueberry").save()`

5.) Override the `__str__` method in the Car model in **models.py** so the query sets can print in a more helpful way (write code below within the Car class, below defining it's attributes):   

> `def __str__(self):`  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`return f'{self.make}, {self.model}'`

\* Changes to a Model don't become active in the shell unless you `exit()` the shell, re-launch, and re-import the models   
\* *Remember:* we made a change to our Model, so we need to **make + apply migrations** (do this before you re-launch your **psql** shell):   

`python manage.py makemigrations`  
`python manage.py migrate`   

6.) Override/update single attribute value of a model instance: assigning it a new value and calling `save()`

> `from main_app.models import Car`   
> `c = Car.objects.first()`   
> `c` --> yields `<Car: Tesla Model X>`   
> `c.make = "Teslaaa"`   
> `c.save()`   
> `c` --> yields `<Car: Teslaaa Model X>`

\* Fourth line of code above --> doesn't just save it the changes to the database - you need to call `save()`.

7.) `Objects.filter()` --> query model's table for data that matches a criteria similar to how we used the Mongoose `find()` method:

`Car.objects.filter(make="Tesla")`   
`<QuerySet[<Car: Tesla Model X>]>`

\* `objects.filter()` or `objects.exclude()` --> similar to WHERE clause in SQL

7.) Query for all cars whose make contains a sub-string:

`Car.objects.filter(make__contains = "K")`   
`<QuerySet[<Car: Kia Stinger>, <Car: Kia EV6>]>`

8.) (Example with Cat model): query for cats with age equal or less than 3:

`Cat.objects.filter(age__lte=3)`

\* Basic lookups: `field__lookup_type = value`   
\* `lte` = "less than or equal to"

9.) `get()` method: common data operation to read one specific model object from the table based on some provided value (usually the id):

`Car.objects.get(id=1)`

\* `get()` can also be called with multiple **field = value** argumentse to query multiple columns

10.) `order_by() method`

`Car.objects.order_by("make")` --> ascending order   
`Car.objects.order_by("-make")` --> descending order   

\* The QuerySet object can be indexed + sliced:

`Cat.objects.order_by("-age")[0]` --> grabs cat who is the oldest of the model instances

11.) Head to **views.py** and import your **Car model**. Then, query for all Car model instances in your **cars_index** view function:

> `from .models import Car`   
> (below: inside cars_index view function, above return render(...)):   
`cars = Car.objects.all()`

****

<br>

### **Django Build-in Admin Features**

\* *superuser:* admin for site   
\* When you are logged-in, you can access the Admin app to add additional users and manipulate Model data

1.) Quit out of the **psql shell** by running `exit()`   
2.) Make sure you are now in your **django environment (django_env)**   
3.) Make sure your server is running by running `python manage.py runserver`   
4.) Run this command:   

`python manage.py createsuperuser`

\* Follow the prompts   
\* Head to **localhost:8000/admin** route in your site to see admin portal   
\* In order to manipulate the Car data in the admin site, we need to "register" the Car model so that the admin portal knows about it. Head to **main_app/admin.py:**   

> `from .models import Car`   
> `admin.site.register(Car)` * under "Register your models here"

****

<br>

### **Car Show Page**

1.) Capture id of car we want details for in the URL   

\* In Django, we use angle brackets to declare a URL parameter to capture values within the segments of a URL as follows":

`cars/<int:car_id>/`

\* The `int` converter --> match + convert captured value from a single string into an integer (in this case). If info in segment does not look like an integer, then it will not be matched.

2.) Clicking on car in **index.html** should trigger request to server to view details of a car:

> `{% for car in cars %}`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<div>`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<a href="/cars/{{ cars.id }}">`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<span>{{ car.make }}</span>`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<span>{{ car.model }}</span>`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<span>Color: {{ car.color }}</span>`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<span>Description: {{ car.description }}</span>`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `</a>`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`</div>`   
> `{% endfor %}`

3.) Add path/route entry to `urlpatterns` list in `main_app/urls.py` that will match each request being made when clicking each individual car:

> `urlpatterns = [`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `...`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `...`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `...`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `path('cars/<int:car_id>/', views.cars_detail, name="detail"),`   
> `]`

4.) Define **cars_detail** view function in **views.py**:

> `def cars_detail(request, car_id):`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`car = Car.objects.get(id=car_id)`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `return render(request, 'cars/detail.html', {'car': car})`

\* Django will pass any captured URL parameters as a named argument to the view function   
\* `car_id` was passed from URL parameter in "detail" path/route (in `urlpatterns` in **main_app/urls.py**)

5.) Render data (car data) within **detail.html** template

\* **cars_detail** view function passed a dictioary of data (context dictionary: `{'car': car}`) to **detail.html** template

6.) Create **detail.html** template:

`touch main_app/templates/cars/detail.html`

7.) Add code to template:

> `{% extends 'base.html' %}`   
> `{% block content %}`   
>    
> `<h1>Car Details</h1>`   
>   
> `<div class="car">`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<div class="car-detail">{{ car.make }}</div>`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<div class="car-detail">{{ car.model }}</div>`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<div class="car-detail">{{ car.color }}</div>`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<div class="car-detail">{{ car.description }}</div>`   
> `</div>`   
>   
> `{% endblock %}`

****

<br>

### **Removing hard-coded URLs in templates**

\* During development, URLs could change   
\* The name argument in our paths/routes in the `urlpatterns` variable (**main_app/urls.py**), is used to obtain the correct URL in templates using DTL's url template tag

