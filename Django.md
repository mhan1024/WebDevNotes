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
