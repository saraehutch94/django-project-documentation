# Django Project: Documentation

### This documentation provides detailed steps on creating a Django project, as well as important information to remember during your build. This documentation also goes over how to connect your Django project to a PostgreSQL database.

---

### In Terminal:

<br>

#### **Create Database**

Create the PostgreSQL database for your project (in terminal):
<br>

`createdb car-collector`

<br>

#### **Head to where your projects are kept:**

Example of where I keep my projects in my computer
<br>

`cd ~/Desktop/` <br>
`cd SEIR/` <br>
`cd HW + Projects/`

<br>

#### **Run command to create Django project:**

This will create a directory with the title of "car-collector"
<br>

`django-admin startproject car-collector`

<br>

#### **Head to `django_env`:**

Running the first command below anywhere in your terminal will take you to your root directory
<br>

`cd ~` <br>
`cd django-env/`

<br>

#### **Enter command to run django environment:**

`pipenv shell`

<br>

#### **Head to your car_collector project:**

`cd ~/Desktop/` <br>
`cd SEIR/` <br>
`cd HW + Projects/` <br>
`cd car-collector/`

<br>

#### **Initialize a Git repository:**

`git init` <br>
`git remote add origin <name of repo link>` <br>
`git remote -v` \* checks if remote was added <br>
`git add -A` <br>
`git commit -m "initialized Django project"` <br>
`git push origin master`

<br>

#### **Open VSCode**

`code .` <br>

<br>

#### **Make sure that your Python Interpreter with django-environment is set up:**

> command + shift + P <br>
> Select Correct Interpreter

Interpreter should look something like this: <br>
`Python 3.9.10 ('django_env-NuObBWWW')`

<br>

#### **Run command to start your Django app directory (in VSCode terminal)**

`django-admin startapp main_app`

 <br>

#### **Install main_app to car_collector:**

1.) Head to `settings.py` under `car_collector` <br>
2.) Find `INSTALLED_APPS` list and add `main_app` to it

<br>

#### **Run server (in VSCode terminal):**

`python manage.py runserver`

**\*We will see migrations warning, but should see successful start to the server.**

<br>

#### **Head to `localhost:8000` in your browser**

You should see a success screen pop-up with a spaceship.
