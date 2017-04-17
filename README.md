# Django-tutorials

# How to Deploy Django 1.10 Framework on Ubuntu 16.04

## Introduction

Django is a free and open-source web framework, written in Python, which follows the model-view-template (MVT) architectural pattern. Django's primary goal is to ease the creation of complex, database-driven websites. Django emphasizes reusability and "pluggability" of components, rapid development, and the principle of don't repeat yourself (DRY). Python is used throughout, even for settings files and data models. Django also provides an optional administrative create, read, update and delete interface that is generated dynamically through introspection and configured via admin models. [1]
Django is a powerful web framework that can help you get your Python application or website off the ground quickly. Django includes a simplified development server for testing your code locally. In this guide we shall see how to install and configure Django into a Python virtual environment using Ubuntu 16.04 as our Operating System. Django will be installed (only) in the project folder of your choice.

## Prerequisites

We will be installing Django within a Python virtual environment. Installing Django into an environment specific to your project will allow your projects and their requirements to be handled separately. Django will be installed only in the project folder that you choose.

## Updating Packages from the Ubuntu Repositories

To get everything we need, update your server's local package index and then install the appropriate packages. We use the next order to update all of our repositories typing:

<pre>
$ sudo apt-get update
</pre>

## Configure a Python Virtual Environment
### What is a Virtual Environment?

Virtualenv is a tool to create Python environments isolated from both libraries and other Python versions, therefore not interfering with Python's default folder. By analogy, Python's virtual environment is to a computer as every single floor to a building. They are provided with some resources such as water or electric energy (these would be our computer). Furthermore, each floor contains their own furniture (being this resource Python's libraries). Now that we have the components from the Ubuntu repositories, we can start working on our Django project. The first step is to create a Python virtual environment so that our Django project will be separate from the system's tools and any other Python projects we may be working on. We need to install the virtualenv command to create these environments. We can get this using pip. If you are using Python 2, type:

<pre>
$ sudo pip install virtualenv
</pre>

If you are using Python 3, type:

<pre>
$ sudo pip3 install virtualenv
</pre>

With virtualenv installed, we can start forming our project. Create a directory where you wish to keep your project and move into the directory:

<pre>
$ mkdir ~/myproject
$ cd ~/myproject
</pre>

Within the project directory, create a Python virtual environment by typing:

<pre>
$ virtualenv myprojectenv
</pre>


This will create a directory called <b>myprojectenv</b> within your <b>myproject</b> directory. Inside, it will install a local version of Python and a local version of pip. We can use this to install and configure an isolated Python environment for our project. Before we install our project's Python requirements, we need to activate the virtual environment. You can do that by typing:

<pre>
$ source myprojectenv/bin/activate
</pre>

Your prompt should change to indicate that you are now operating within a Python virtual environment. It will look something like this: 

<pre>
(myprojectenv)user@host:~/myproject$
</pre>

With your virtual environment active, install Django with the local instance of pip:

<pre>
$ pip install django
</pre>

<i>Note: Regardless of whether you are using Python 2 or Python 3, when the virtual environment is activated, we should use the pip command (not pip3).</i>


## Create and Configure a New Django Project

Now that Django is installed in our virtual environment, we can create the actual Django project files.

### Create the Django Project

Since we already have a project directory, we will tell Django to install the files here. It will create a second level directory with the actual code, which is normal, and place a management script in this directory. The key to this is the dot at the end that tells Django to create the files in the current directory:

<pre>
$ django-admin.py startproject myproject .
</pre>

### Adjust the Project Settings

The first thing we should do with our newly created project files is adjust the settings. Open the settings file with your text editor:

<pre>
$ nano myproject/settings.py
</pre>

We are going to be using the default SQLite database in this guide for simplicity's sake, so we don't actually need to change too much. We will focus on configuring the allowed hosts to restrict the domains that we respond to and configuring the static files directory, where Django will place static files so that the web server can serve these easily. Begin by finding the ALLOWED_HOSTS line. Inside the square brackets, enter your server's public IP address, domain name or both. Each value should be wrapped in quotes and separated by a comma like a normal Python list:


### ~/myproject/myproject/settings.py

<pre>
. . .
ALLOWED_HOSTS = ["server_domain_or_IP"]
. . .
</pre>

At the bottom of the file, we will add a line to configure this directory. Django uses the STATIC_ROOT setting to determine the directory where these files should go. We'll use a bit of Python to tell it to use a directory called "static" in our project's main directory:

### ~/myproject/myproject/settings.py

<pre>
. . .
STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, 'static/')
</pre>

BASE_DIR will be the path to the project folder, for example:

<pre>
/home/user/myproject/myapp/
</pre>

Save and close the file when you are finished. In order to improve our project's structure it is recommended to create a folder where all our webpage templates will be stocked. We shall call it 'templates' and place it into 'myproject' folder. This folder will be referenced within the same file ‘settings.py’ in “DIRS” section and using BASE_DIR again as path to our project folder, as follows:

<pre>
TEMPLATES = [
{
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, 'templates')],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
</pre>

Now, will be to start our application:

<pre>
$ cd ..
$ python manage.py startapp myapp
</pre>

We must add into INSTALLED_APPS our application name like following:

<pre>
$ cd myproject
$ nano settings.py
</pre>

<pre>
INSTALLED_APPS = (
'django.contrib.admin',
'django.contrib.auth',
'django.contrib.contenttypes',
'django.contrib.sessions',
'django.contrib.messages',
'django.contrib.staticfiles',
‘myapp’,
)
</pre>

### Complete Initial Project Setup

Now, we can migrate the initial database schema to our SQLite database using the management script:

<pre>
$ cd ~/myproject
$ ./manage.py makemigrations
$ ./manage.py migrate
</pre>

Create an administrative user for the project by typing:


<pre>
$ ./manage.py createsuperuser
</pre>

You will have to select a username, provide an email address, and choose and confirm a password. We can collect all of the static content into the directory location we configured by typing:

<pre>
$ ./manage.py collectstatic
</pre>

You will have to confirm the operation. The static files will be placed in a directory called static within your project directory. Finally, you can test your project by starting up the Django development server with this command:

<pre>
$ ./manage.py runserver 0.0.0.0:8000
</pre>

In your web browser, visit your server's domain name or IP address followed by :8000:

http://server_domain_or_IP:8000 

You should see the default Django index page:



If you append /admin to the end of the URL in the address bar, you will be prompted for the administrative username and password you created with the createsuperuser command:

After authenticating, you can access the default Django admin interface:



### Models


A model is a structure that how will be storage the data in our database. Will be to precise the field type like integer, float, string, array, files, etc. A simple model looks like this:


### ~/myproject/myapp/models.py

<pre>
from django.db import models
class first_model (models.Model):
	field1= models.CharField(max_length=200)
	field2 = models.BooleanField(default=False)
	field3 = models.FileField(upload_to=”static/img”)
	field4 = models.IntegerField(null=True)
	...
</pre>

<i>
Note: Any change that you do in models.py, requires a “makemigrations” and “migrate” order to upload our database:
</i>

<pre>
$ python manage.py makemigrations
$ python manage.py migrate
</pre>

### URLs controller

Into 'myproject' folder we shall edit urls.py file. This file will manage all request and will take us to appropiate view. If someone wants to access /index, 'urls.py' will take us to index view. The process follows this steps:

### ~/myproject/myproject/urls.py

<pre>
from django.conf.urls import include, url
from django.contrib import admin

urlpatterns = [
	url(r’^admin/’, include(admin.site.urls))
	url(r’’, include(‘myapp.urls’)),
]
</pre>

Save and close the file and go to:

<pre>
$ cd ..
$ cd myapp/
$ sudo nano urls.py
</pre>

We create the urls.py file inside our app:

### ~/myproject/myapp/urls.py

<pre>
from django.conf.urls import include, url
from django.contrib import admin
from . import views
from django.conf.urls.static import static

admin.autodiscover()

urlpatterns = [
	url(r’^$, views.index),
]
</pre>

<i>Note: If you want to see more regular expressions (regex), can visit the following link:
</i>

* <a href="https://docs.djangoproject.com/en/1.10/topics/http/urls/"> Regular Expressions


### Views

A views return an order that client side request (response with a template or request to our database, etc). In the same folder, we shall configurate the views.py file. A simple view that serves a template, looks like this:

<pre>
~/myproject/myapp/views.py
</pre>

<pre>
from django.shortcuts import render

function index (request):
	return render(request,’myapp/index.html’, {})
</pre>

<i>Note: For that one template show static files, it must content in the beginning a sentence like this:
</i>

<pre>
{% load staticfiles %}
</pre>

If don't, none will be showed any static file

When you are finished exploring, hit CTRL-C in the terminal window to shut down the development server. We're now done with Django for the time being, so we can back out of our virtual environment by typing:

<pre>
$ deactivate
</pre>

### Wrapping Up Some Permissions Issues for Apache servers.

If you are using the SQLite database, which is the default used in this article, you need to allow the Apache process access to this file. To do so, the first step is to change the permissions so that the group owner of the database can read and write. The database file is called db.sqlite3 by default and it should be located in your base project directory:

<pre>
$ chmod 664 ~/myproject/db.sqlite3
</pre>

Afterwards, we need to give the group Apache runs under, the www-data group, group ownership of the file:

<pre>
$ sudo chown :www-data ~/myproject/db.sqlite3
</pre>

In order to write to the file, we also need to give the Apache group ownership over the database's parent directory:

<pre>
$ sudo chown :www-data ~/myproject
</pre>


### References

<a href="https://www.digitalocean.com/community/tutorials/how-to-serve-django-applications-with-apache-and-mod_wsgi-on-ubuntu-16-04"> [1] - DigitalOcean

<a href="https://en.wikipedia.org/wiki/Django_(web_framework)"> [2] - Wikipedia

<a href="https://www.djangoproject.com/start/"> [3] - DjangoProject
