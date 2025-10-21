# Django
A Python web framework for building websites or web applications
- Emphasis on component reusability (DRY - don't repeat yourself)

## Design Pattern (MVT)
- Model: data to be presented from the database
  - Located in `models.py`
  - When making changes to this file, be sure to run this command so that the plan for the database is created in migration files\
    `python3 manage.py makemigrations <model_name>`
  - Apply the changes to the database\
    `python3 manage.py migrations`
- View: functions that get the data and handles the logic so it can be passed into a template
  - Located in `views.py` 
- Template: a file that handles what users see and how results should be presented
  - Located in a folder named `templates`
 
## URLs
They connect web addresses to views
- Located in `urls.py`
In `urls.py`, the general syntax is:
```python
  from django.urls import path
  from . import views # imports views files

  urlpatterns = [
      path('route/', views.view_name, name='route_name')
  ]
```
  
## Set Up
- Create or navigate to project directory\
  `cd <project-directory>`
- Create a virtual environment\
  `python3 -m venv venv`
- Activate the virtual environment\
  `source venv/bin/activate`
- Install Django inside the environment\
  `python3 -m pip install django`
- Create project\
  `django-admin startproject <project_name> .`

## Running Project
- Create or navigate to project directory\
  `cd <project-directory>`
- Activate the virtual environment\
  `source venv/bin/activate`
- Run project\
  `python3 manage.py runserver`

## Apps
An app is a web app that handles one specific feature or purpose
- Create an app
  `python3 manage.py startapp <app_name>`
- Add app to `settings.py`'s `INSTALLED APPS = [ ..., <app_name> ]`

## Connecting to PostgreSQL
- Login as postgres\
  `psql -U postgres`
- Create a user
  ```sql
    CREATE USER <project_user> WITH PASSWORD '<password>';
  ```
- Create a database
  ```sql
    CREATE DATABASE <project_db> OWNER <project_user>;
  ```
- Grant privileges
  ```sql
    ALTER ROLE <project_user> SET client_encoding TO 'utf8';
    ALTER ROLE <project_user> SET default_transaction_isolation TO 'read committed';
    GRANT ALL PRIVILEGES ON DATABASE <project_db> TO <project_user>;
  ```
- Update Django `settings.py`
  ```python
    DATABASES = {
      "default": {
          'ENGINE': 'django.db.backends.postgresql',
          'NAME': '<project_db>',
          'USER': '<project_user>',
          'PASSWORD': '<password>',
          'HOST': 'localhost',
          'PORT': '5432',
      }
    }
  ```

## Connecting to Firebase
- Obtain the Firebase Admin SDK by going to <a href="https://firebase.google.com/?gclsrc=aw.ds&gad_source=1&gad_campaignid=12211052842&gbraid=0AAAAADpUDOgjkaEAAY1Df3S7qeMlE4ZwJ&gclid=CjwKCAjwu9fHBhAWEiwAzGRC_0lJvSlqrJ8P6moempR3H1R9dY5Oo3wL9Xxkn8cuV9JS8KaBBqMhghoCLW4QAvD_BwE">Firebase Console</a>
- Open your project and click on the ⚙️ 'Settings' icon -> Project Settings
- In Project Settings, go to the 'Service Accounts' tab and click on the 'Generate new private key' and download the `serviceAccountKey.json`
- Install Firebase Admin SDK\
  `python3 -m pip install firebase-admin`
- Place it in the project's directory
  ```
  project_root/
  ├─ backend/
  │  ├─ serviceAccountKey.json
  │  ├─ settings.py
  ```
- Initialize Firebase in `settings.py`
  ```python
  import firebase_admin
  from firebase_admin import credentials
  
  cred = credentials.Certificate("path/to/serviceAccountKey.json")
  firebase_admin.initialize_app(cred)

  ```

## Models
- To create a model, make sure you have an app created and navigate to the `models.py` file in the `/<app_name>/` folder
- Add a table by creating a class and describing the table's fields
  ```python
  from django.db import models

  class <table_name>(models.Model):
    <field> = models.CharField(max_length=255) # this is a text field with a max length of 255 characters
  ```
- After describing a Model, run this command to prepare the migration files describing changes \
  `python3 manage.py makemigrations <app_name>`
- Note: Django automatically inserts the `id` field that is also auto-incremented
- Run this command so that Django will apply those migrations to the actual database\
  `python3 manage.py migrate`
