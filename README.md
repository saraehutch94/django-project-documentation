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

1.) In **index.html**, fix code:   

~~`<a href="/cars/{{ car.id }}">`~~   
`<a href="{% url 'detail' car.id %}">`

\* `url` --> declaring "url template tag"   
\* `detail` --> name attribute of path   
\* `car.id` --> data to pass to path as URL parameter   

****

<br>

### **Django Class-Based Views (CBVs)**

\* CBVs can accomplish full CRUD   
\* classes instead of functions to define views   
\* defines view functionality   
\* can be used in lieu of view functions (NOT to fully replace --> can and WILL coexist)   
\* allows us to abstract away from functions   
\* CBVs can ask database for model instance, render a template for us, will provide all the data to that template   
\* if there is a form on the template, it will validate the data from the form, as well as create the actual form   
\* Example:   

> `from django.views.generic import ListView`   
>   
> `class BookList(ListView):`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`model = Book`

\* Need to define Model you want to work with in each CBV   
\* Whenever we reference a class-based view inside our **urls.py**, we have to invoke a special class method called `as_view()` --> returns all functionality needed to facilitate a view (activates CBV)   
\* **CBVs save time!**   
\* **Important note about CBVs:** You can create an index and detail page using CBVs, by utilizing this code:

> `from django.views.generic.list import ListView`   
> `from django.views.generic.detail import DetailView`
>   
> `class CarList(ListView):`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`model = Car`   
>   
> `class CarDetail(DetailView):`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`model = Car`

1.) Create a new instance of the Car model using Django's built-in **CreateView** in **main_app/urls.py**:

> `urlpatterns = [`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`...`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`path('cars/create/', views.CarCreate.as_view(), name='cars_create'),`   
> `]`

\* Believe it or not, this one route is used for both **GET** and **POST** requests   
\* Since Django **does not** practice RESTful routing, this can happen (both provides form for creating car as well as creates new car data instance)    

2.) Head to **base.html** to add link to creating a car in nav bar:   

`<li><a href="{% url 'cars_create' %}">Create Car</a></li>`   

3.) Head to **views.py** to define CBV for creating a car:   

> `from django.views.generic.edit import CreateView`   
>    
> `class CarCreate(CreateView):`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`model = Car`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`fields = '__all__'`

\* `Car` --> Model we want to create instance of   
\* `fields` --> individual columns in database; here we are telling Django which fields we want to provide in the form Django creates for us   
\* If you didn't want to include all fields in the form, you can write your fields attribute in a tuple, like this:   

`fields = ('make', 'model', 'color', 'description')`

\* If you test the create car link on the nav, you should see an error in browser: **The form template has not been created yet**   
\* If you noticed in this error, Django is expecting a certain file named **car_form.html** to be within a folder called **main_app** within the **templates** folder   
\* *Django is a convention over configuration framework*. Meaning, it expects for things to be named a certain way so they can be found by Django correctly

4.) Head to templates directory. Create a folder called **main_app** and a file inside that folder called **car_form.html**   

`mkdir main_app/templates/main_app`   
`touch main_app/templates/main_app/car_form.html` --> takes model name and adds __form to it

5) Add **car_form.html** template content

> `{% extends 'base.html' %}`   
> `{% block content %}`   
> `<h1>Add a new Car</h1>`   
>    
> `<form action="" method="POST">`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`{{ form }}`   
> `</form>`   
> `{% endblock %}`

\* `form` --> placeholder to print form w/ fields   
\* Django will try to add form fields to HTML5 form, so we need to add `<form>` tag

6.) Add submit button + **csrf token** to form:

> `<form action="" method="POST">`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`{% csrf_token %}`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`{{ form }}`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<input type="submit" value="Add Car"/>`   
> `</form>`   
> `{% endblock %}`

\* *csrf:* "cross-site request forgery"   
\* Django's forms are super secure, but they also take advantage of an extra layer of security that fights against an exploitation technique called **csrf**   
\* A hacker will build a website clone and present it to people, have people enter in their information; the hacker will then go into the real site and use those credentials used on their fake site to log into the real site   
\* Django creates special token inside form that, before it gets submitted, Django has to validate the token's authenticity before allowing it to penetrate the server   
\* Can see the token if you open Chrome DevTools + click anywhere in the form

\* **Should get an error when submitting new car --> in Exception Value of error, it will state that there is no URL to redirect to (provide a URL or define get_absolute_url on model**

\* Always check **Exception Value** in browser error for accurate reasoning on why error is happening

\* **get_absolute_url** - preferred method; Django allows us to define a special method for every instance of a model that will be called by the instance whenever it is created; this method is a method that each car object can call on itself and can tell it to redirect us to anywhere in the app (show, index, show, etc.)

7.) Head to **models.py** and add **get_absolute_url** method to the Car model:

> `def get_absolute_url(self):`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`return reverse('detail', kwargs={'car_id': self.id})`

\* `'detail'` --> where you want to send user after they created a car

\* Code above performs "reverse lookup" --> process of starting w/ model instance + trying to look up url path for that model instance (past: started w/ url path + tried to find model instance that path belongs to); now it's reversed

\* This method gets invoked when creating the object. Once object is created, we will do reverse lookup + find corresponding url path (ex: 'detail' path) that belonds to that model instance by using the **reverse function**, which uses **kwargs** to insert self.id (model instance's id) into path query param (ex: car_id)

\* Even with this method being built into Django, it is not defined until we define it

\* **Fat Models, Skinny Controllers** --> we want to have as much functionality as possible in models, while keeping out controller code short + sweet

\* *non-preferred method:*

`success_url = '/cars/'` --> after car is created, I want to be redirected back to all cars (index page)

\* This method can't be avoided when it comes to deleting a model instance (can't be brought to detail page after deleting if there is no more car to display in detail page - will see later on in docs)

****

<br>

### **Update + Delete**

1.) Define update + delete routes in **main_app/urls.py**

> `urlpatterns = [`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`...`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`path('cars/<int:pk>/update/', views.CarUpdate.as_view(), name='cars_update'),`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`path('cars/<int:pk>/delete/', views.CarDelete.as_view(), name='cars_delete'),`   
> `]`

\* CBVs don't use _id convention in their path/URL parameters; they use **pk/primary key**, which is the same as id

\* In the database, pk doesn't exist (just a naming convention) --> in the database, it's id

\* pk becomes variable that CBV will be looking for so when database lookup occurs, it will use pk to find the object

2.) Head to **detail.html** template for cars to add links for editing and deleting (under second div with all content):

> `<div class="card-action">`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<a href="{% url 'cars_update' car.id %}">Edit</a>`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<a href="{% url 'cars_delete' car.id %}">Delete</a>`   
> `</div>`

\* `class="card-action"` --> from Materialize CSS framework (check out their docs)

3.) Head to **views.py**; next to `from django.views.generic.edit import CreateView`, add:

`, UpdateView, DeleteView`

4.) Define **UpdateView** class (still within **views.py**):

> `class CarUpdate(UpdateView):`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`model = Car`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`fields = '__all__'`

And **DeleteView** class:

> `class CarDelete(DeleteView):`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`model = Car`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`success_url = '/cars/'`   

\* Mentioned earlier in the docs, we talked about how the **DeleteView** would not be able to utilize the method `get_absolute_url`. This is because once a model instance is deleted, it cannot be sent back to it's detail page. Therefore, we need to use the `success_url` attribute in it's class, so we can send the user back to the index page once the model instance is deleted.

\* When you press the edit button we added to each car model instance, it will bring you to the "Add a Car" form. You can edit the car using this form, but the important part here is that Django takes the same form created earlier for creating a car and uses it for editing a car as well. We want to add some template logic so we can differentiate between the two forms.

5.) Head to the **car_form.html** template, so we can add the logic:

> `{% if object %}`   
> `<h1>Edit {{ object.name }}</h1>`   
> `{% else %}`   
> `<h1>Add a Car</h1>`   
> `{% endif %}`   
>   
> `...`
>   
> `<input class="btn blue" type="submit" value="{% if object %} Edit {% else %} Add {% endif %}"/>`   
>    
> `{% if object %}`   
> `<a href="{% url 'detail' object.id %}">Cancel</a>`   
> `{% else %}`   
> `<a href="{% url 'index' %}">Cancel</a>`   
> `{% endif %}`   

\* `object` --> default variable name: Django view engine uses this variable to reference the object that we are editing on the page (if it exists --> editing object / if it does not exist --> adding a car/no object created yet)

\* If you try to delete, you will get an error in the browser (Exception Value): **Django is looking for a deletion confirmation template** --> **"main_app/template/car_confirm_delete.html** 

6.) Add **car_confirm_delete.html** template to **template** folder:

`touch main_app/templates/car_confirm_delete.html`

Add content to template:

> `{% extends 'base.html' %}`   
> `{% block content %}`   
>   
>  `<h3>Are you sure you want to remove {{ object.make }} {{ object.model }}?</h3>`   
>   
> `<form action="" method="POST">`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`{% csrf_token %}`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`{{ form.as_p }}`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<input type="submit" value="Yes - Remove"/>`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<a href="{% url 'detail' object.id %}">Cancel</a>`   
> `</form>`   
>   
> `{% endblock %}`   

\* `action` attribute in `<form>` tag --> where to send form data when form is submitted   
\* No `action` attribute --> browser automatically adopts same url in address bar (same URL, but POST request)   
\* You should now be able to delete a model instance from the database

****

<br>

### **Adding another model to the database**

\* These will be the same steps as the Car Model, except you create a new model, make + apply migrations, and start adding CRUD to the Model.   
\* For this Model, we will apply the `ListView` and `DetailView` CBVs

1.) Create model in **main_app/models.py**

> `class Tree(models.Model):`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`scent = models.CharField(max_length=100)`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`color = models.CharField(max_length=100`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`description = models.CharField(max_length=250)`   
>   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`def __str__(self):`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`return self.scent`

2.) Make + apply migrations

`python manage.py makemigrations`   

\* You should see **Create model Tree**

`python manage.py migrate`

\* You should see new Tree table in database if you run these commands now:

`\l`   
`\c car_collector`   
`\d`

3.) Add Tree model instances to database:

`python manage.py shell`  
`from main_app.models import Tree`   
`Tree(scent="Black Ice", color="black", description="The original Little Tree car scent").save()`

\* Do this a few more times to add more Tree model instances. Check to make sure they were added by typing `Tree.objects.all()` into terminal

4.) Quit shell using `quit()`; now time to add index route:

**in main_app/urls.py - urlpatterns**

`path('trees/', views.TreeList.as_view(), name='tree_index',`

5.) Add link to Tree index in **base.html** nav bar:

`<li><a href="{% url 'tree_index' %}">View All Little Trees</a></li>`

6.) Import **ListView** and **DetailView** to **views.py**

`from django.views.generic.list import ListView`   
`from django.views.generic.detail import DetailView`   

7.) Create **CBV** for **ListView**:

> `class TreeList(ListView):`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`model = Tree`

8.) Create appropriate template in **main_app** folder and add content:

`touch main_app/templates/main_app/tree_list.html`
   
> `{% extends 'base.html' %}`   
> `{% block content %}`
>   
> `<h2>All Little Trees</h2>`   
>   
> `{% for tree in object_list %}`   
> `<div class="card">`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<a href="{% url 'trees_detail' tree.id %}">`
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<div class="card-content">`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<span class="card-title">{{ tree.scent }}</span>`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`</div>`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`</a>`   
> `</div>`   
>   
> `{% endfor %}`   
> `{% endblock %}`

9.) Add path for **detail** page in **main_app/urls.py - urlpatterns**:

`path('/cars/<int:pk>/', views.TreeDetail.as_view(), name='tree_detail'),`   

10.) Add CBV for **detail** route/path:

> `class TreeDetail(DetailView):`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`model = Tree`   

11.) Add template for **tree_detail** path + add content:

`touch main_app/templates/main_app/tree_detail.html`


> `{% extends 'base.html' %}`   
> `{% block content %}`   
>   
> `<h2>{{ object.scent }} Details</h2>`   
>   
> `<div class="card">`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<div class="card-content"></div>`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<span class="card-title">{{ object.scent }}</span>`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<p class="tree-detail">Color: {{ object.color }}</p>`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<p class="tree-detail">Description: {{ object.description }}</p>`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<div>`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<div class="card-action">`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<a href="{% url 'trees_update' object.id %}" class="card-action-link">Edit</a>`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<a href="{% url 'trees_delete' object.id %}" class="card-action-link">Delete</a>`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`</div>`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`</div>`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`</div>`   
>   
> `{% endblock %}`

\* **Rest of CRUD:** review how we implemented CBVs for the Car Model (create, update and delete operations)

***

<br>

### **One-To-Many Relationships with Django Models**

1.) Add another model in **models.py** that will include a **foreign key**, referencing each car model instance:

> `class Gas(models.Model):`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`date = models.DateField()`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`fill = models.CharField(max_length=1)`   

\* The reason behind the `max_length=1` in the **fill** attribute above, is because we want to take up as little space as possible; this is very common (especially in SQL) to represent data with one or two characters (ex: a **Boolean** field: **t = True**, **f = False**; OR **0 = True**, **1 = False**)   

2.) Define **FIELD** choices (still within **models.py**):

> `FILLS = (`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`('1', 'Week 1'),`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`('2', 'Week 2'),`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`('3', 'Week 3'),`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`('4', 'Week 4'),`   
> `)`

\* The numerical values above represent the values that will be stored in the database   
\* The 'Week' values are the human-readible representations of the data   
\* Defining an **uppercase variable:** defining a constant; this is a great use case for a tuple --> in Python, we store information that won't change and won't take up a lot of space inside tuples

3.) Let model know that we have the **FIELD** choices tuple   
\* Options the user will be able to choose from whenever they are creating a fill for a car

(in **Gas model**):

`fill = models.CharField(max_length=1, choices=FILLS, default=FILLS[0][0]`

\* Defining the `choices` keyword argument on the `fill` attribute unlocks more functionality that comes with Django models (ex: `self.get_fill_display()`)   
\* `default = FILLS[0][0]` --> this is pointing to the '1' value in the first tuple of the `FILLS` tuple --> sets default choice

4.) Make sure the representation of model instances are human-friendly (still within **Gas Model**):

> `def __str__(self):`   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`return f'{self.get_fill_display()} on {self.date}'`

\* `get_fill_display()` --> this special method automatically gets created whenever we have an **attribute with a choices keyword argument**