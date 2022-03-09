# Django Project: Documentation

### This documentation provides detailed steps on creating a Django project, as well as important information to remember during your build. This documentation also goes over how to connect your Django project to a PostgreSQL database.

---

<br>

### **Terminal: Create Database**

Create the PostgreSQL database for your project:
<br>

`createdb car-collector`

<br>

### **Terminal: Head to where your projects are kept:**

Example of where I keep my projects in my computer
<br>

`cd ~/Desktop/` <br>
`cd SEIR/` <br>
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

`cd ~` <br>
`cd django-env/`

<br>

### **Terminal: Enter command to run django environment:**

`pipenv shell`

<br>

### **Terminal: Head to your car_collector project:**

`cd ~/Desktop/` <br>
`cd SEIR/` <br>
`cd HW + Projects/` <br>
`cd car-collector/`

<br>

### **Terminal: Initialize a Git repository:**

`git init` <br>
`git remote add origin <name of repo link>` <br>
`git remote -v` \* checks if remote was added <br>
`git add -A` <br>
`git commit -m "initialized Django project"` <br>
`git push origin master`

<br>

### **Terminal: Open VSCode**

`code .` <br>

<br>

### **Make sure that your Python Interpreter with django-environment is set up:**

> command + shift + P <br>
> Select Correct Interpreter

Interpreter should look something like this: <br>
`Python 3.9.10 ('django_env-NuObBWWW')`

\* May need to shut down terminal and re-start to see django environment pop up.

<br>

### **VSCode Terminal: Run command to start your Django app directory**

`django-admin startapp main_app`

 <br>

### **Install main_app to car_collector:**

1.) Head to `settings.py` under `car_collector` <br>
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

1.) Find `DATABASES {}` dictionary <br>
2.) Change: <br>
`'NAME': BASE_DIR / 'db.sqlite3',` to <br>
`'NAME': 'car_collector',` \* database name/name of project

<br>

### **Transfer migrations to database:**

**Migrations:** ways to keep track of changes made to databases overtime --> especially changes made to models (will be covered later in documentation)

1.) Shut down terminal using **control + C** <br>
2.) Run command: `python manage.py migrate`

\* In beginning of project: the migrations represent all of the initial changes we need to make in order to create database (even though database is already created, tables need to be added to database) <br> \* These tables are put in place to manage different parts of our project internally (manage various utilities behind the scenes) <br> \* Migrate = transferring from file to database system

<br>

### **Connect core of project to Django app**

\* Core project = `car_collector` <br> \* Django app = `main_app`

1.) In VSCode terminal, add file to `main_app` called `urls.py`: <br>
`touch main_app/urls.py` <br>
2.) Need to connect `urls.py` "urls config" in `main_app` to `urls.py` "urls config" in `car_collector`:

![Components of Django Project](https://i.imgur.com/gscrXbn.png)

- Head to `urls.py` in core project (`car_collector`); under `urlpatterns []` list, add: <br>

> `from django.urls import path, include` <br>
> <br>
> `urlpatterns = [` <br>
> ... <br>
> `path('', include('main_app.urls')),` <br>
> `]`

\* `''` above: root url path (empty string) <br>
\* for the root path, include Django app (`main_app`) and `urls.py` file

