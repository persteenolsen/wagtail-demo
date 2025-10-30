

# Python + Django + Wagtail CMS + MySQL + Vercel

This example shows how to use Wagtail CMS which is build of top of Django 5 on Vercel with Serverless Functions using the [Python Runtime](https://vercel.com/docs/concepts/functions/serverless-functions/runtimes/python).

Last updated 30-10-2025

Node version selected at Vercel Cloud: 22

## Demo at Vercel

https://wagtail-demo.vercel.app/

## Installing

- Download Python from the official website [Python](https://python.org/)
- Make sure that you have installed Python by the command in Powershell: "python --version"
- Download the Python extension for Visual Studio Code which automatically include the Pylance extionsion
- Download / fork this Django Starter Website from my GitHub
- Create the virtual envirement ".venv" for the Django Web App by Powershell or by VS Code
- Virtual Enviroment by VS Code: "View - Command Palette - Python Create Enviroment"


## Install by Python commands in Powershell at Windows 10

- python -m venv .venv

- cd .venv

- Scripts/activate

- Copy requirements.txt to .venv

- (.venv) pip install -r requirements.txt

- (.venv) pip freeze > requirements.txt

- (.venv) cd ../

- (.venv) python manage.py runserver

When starting the Django Website from the Vertual Enviroment (.venv) you will notice that Django 5.2 and Wagtail 7.0 will be in use. Otherwise you can use the Global Django and Wagtail if you have one installed by running:

- python manage.py runserver

## How it Works

The Wagtail and Django application, `home` is configured as an installed application in `vercel_app/settings.py`:

```bash
# vercel_app/settings.py
INSTALLED_APPS = [
    # ...
    'home',
]
```

We allow "\*.vercel.app" subdomains in `ALLOWED_HOSTS`, in addition to 127.0.0.1:

```bash
# vercel_app/settings.py
ALLOWED_HOSTS = ['127.0.0.1', '.vercel.app']
```

The `wsgi` module must use a public variable named `app` to expose the WSGI application:

```bash
# vercel_app/wsgi.py
app = get_wsgi_application()
```

The corresponding `WSGI_APPLICATION` setting is configured to use the `app` variable from the `vercel_app.wsgi` module:

```bash
# vercel_app/settings.py
WSGI_APPLICATION = 'vercel_app.wsgi.app'
```

This Wagtail and Django example uses the Web Server Gateway Interface (WSGI) with Django to enable handling requests on Vercel with Serverless Functions.

## Routing 

There are severals views in `home` which load HTML Django Templates `templates`:

The views are exposed a URL through `urls.py`:

Finally, it's made accessible to the Django server inside `vercel_app/urls.py`:

```bash
# vercel_app/urls.py
from django.urls import path, include

urlpatterns = [
    ...
    path('', include('home.urls')),
]
```

## Templates

To use build in templates like base.html create the dir 'templates' at root level and put the HTML files there

Tell Django where to look for Templates by `settings.py`:

Find the section TEMPLATES = [] and add the dir of the Templates

'DIRS': [BASE_DIR / 'templates']

## Running Locally

```bash
python manage.py runserver
```
Your Django application is now available at `http://127.0.0.1:8000/`.

## The Admin Backend and Databases

The Admin Backend is using a MySQL Database for both Dev + Prod, but is able to use a SQLite for Dev

To connect to the MySQL install the Python package "pymysql" and the packages from the requirements.txt

```bash
pip install -r requirements.txt
```

Take a look at the files needed for connecting to MySQL: `vercel_app/mysql_setup.py` and 

`vercel_app/__init__.py`

Create a Super User for the Admin Backend in the MySQL

```bash
python manage.py createsuperuser
```

Make the Migration to the MySQL DB

```bash
python manage.py makemigrations
python manage.py migrate
```
You will need to do the Migration at first and when / if you will add, update or delete models.py which this Django Web App does not use

For using a SQLite developing / locally make the config in the setting file `vercel_app/settings.py`

Find the section DATABASES = {} and add support for SQLite and comment out the MySQL

## Static files for the Frontend

Make sure that the Python package 'whitenoise' is installed from the requirements.txt

Note: Make sure you have the line 'whitenoise.middleware.WhiteNoiseMiddleware' in the 

MIDDLEWARE = [] at the `vercel_app/settings.py` along with the other packages

Finally, take a look at `vercel_app/settings.py`:

Find where Django now looks for the static files

STATIC_URL = 'static/'

Where you put your static files in the dir 'static'

STATIC_ROOT = BASE_DIR/'static' 

## Additional directory from which to load static files if wanted

The files in the dir 'asset' will be copied to the dir 'static' after running

```bash
python manage.py collectstatic
```

## Check that Wagtail and Django is serving static files by URL

Type the URL in your Browser after deployment to Vercel

https:// your project at vercel.app/static/pso-django.jpg

or the URL when running locally

`http://127.0.0.1:8000/static/pso-django.jpg`

If everything is fine my photo will be displayed

The CSS files can be tested the same way like the .jpg above

Now you can use the images and CSS in your Templates

## Running Locally and take a look at the Website

```bash
python manage.py runserver
```

The Django application is now available at `http://127.0.0.1:8000`

## Deployment to Vercel

Make sure that your static files are ready by running

```bash
python manage.py collectstatic
```

Note: The above command may not be needed as this Starter dont have a Django Admin backend

Take a look at the file `vercel.json`

Make sure to set Debug = False in the file `vercel_app/settings.py`

Make a commit to your GitHub and your Django will build and deploy

## Models

Add the simple Model "Home" to be administrated by the Admin Backend and displayed by the Frontend.

- Create a file `home/models.py` with your new Model Home

- Run the command:

```bash
python manage.py makemigrations example
```
Note: It is important to add the name of the app in the command `home` !!!

This command will create a file for the migration of the Model to a Table in the MySQL DB

- Now run the command: 

```bash
python manage.py migrate
```
This will create the Home in the DB and you are now ready for administrate the Home Page by the Wagtail Admin Backend

Happy use of Django with Wagtail CMS :-)
