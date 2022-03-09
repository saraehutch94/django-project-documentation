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

### **Terminal: Head to `django_env`:**

Running the first command below anywhere in your terminal will take you to your root directory
<br>

`cd ~`   
`cd django-env/`

<br>

### **Terminal: Enter command to run django environment:**

`pipenv shell`

<br>

### **Terminal: Head to your car_collector project:**

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

### **Install main_app to car_collector:**

1.) Head to `settings.py` under `car_collector`    
2.) Find `INSTALLED_APPS` list and add `main_app` to it

<br>

### **VSCode Terminal: Run server:**

`python manage.py runserver`

**\*We will see migrations warning, but should see successful start to the server.**

<br>

### **Head to `localhost:8000` in your browser**

You should see a success screen pop-up with a spaceship.

<br>

### **Head to `settings.py` under `car_collector`:**

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

\* Core project = `car_collector` --> project's entry point (similar to server.js in Express but with multiple files)   
\* Django app = `main_app` --> controllers, models, anything specific to routing goes here

1.) In VSCode terminal, add file to `main_app` called `urls.py`:    
`touch main_app/urls.py`   
2.) Need to connect `urls.py` "urls config" in `main_app` to `urls.py` "urls config" in `car_collector`:

![Components of Django Project](https://i.imgur.com/gscrXbn.png)

- Head to `urls.py` in core project (`car_collector`); under `urlpatterns []` list, add:   

> from django.urls import path, include   
>    
> urlpatterns = [   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ...   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; path('', include('main_app.urls')),   
> ]

\* `''` above: root url path (empty string)   
\* for the root path, include Django app (`main_app`) and `urls.py` / "urls config" file

<br>

### **Define URL patterns in `urls.py` inside `main_app`:**

> from django.urls import path   
> 
> urlpatterns = [   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; path('', views.home, name='home'),   
> ]

\* All `urls.py` modules need this variable inside of them --> Django will scan file and look for variable and any url paths defined in that list variable   
\* *path:* method used to create url paths; generates and allows us to define url paths   
\* `''` above: Home page/url path   
\* `views.home` --> functionality we want to invoke in result of path being requested (home view function in `views.py`)   
\* `name='home'` --> alias for url pattern; instead of referencing path itself, we can reference name attribute of path whenever we are trying to create dynamic paths in our templates (will be shown later in documentation)

<br>

### **Head to `views.py` in `main_app` (controllers file):**

\* *view functions* are invoked in response to a request being made to server (heading to a specific path)

1.) Bring controller into scope:   
`from . import views`   

2.) Add home views function:
> from django.http import HttpResponse    
>
> def home(request):   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return HttpResponse("hello")    

\* `return` --> response of request   
\* `request` --> similar to requires in Express, but just putting request instead    
\* Try refreshing the page; you should see "hello world" in browser window   


### **Write similar view functions for your about page and any other pages (define path as well in `urls.py` in `main_app`)**