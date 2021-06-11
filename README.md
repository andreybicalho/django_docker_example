# django_docker_example

Simple docker tutorial for python projects using django.

## 1
* Django setup
```
docker-compose run web django-admin startproject djangoProjExample .
```

## 2
* open `djangoProjName/settings.py` file and change the database configs accordingly (see db service in `docker-compose.yaml`):
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'postgres-db',
        'USER': 'postgres',
        'PASSWORD': 'postgres',
        'HOST': 'db',
        'PORT': 5432,
    }
}
```

and run:
```
docker-compose up
```

## 3
* fix missing depencies in `requirements.txt` by adding the `psycopg2-binary>=2.8` dependency and run:
```
docker-compose build
```

if for some reason djangoProjExample gets root permision change the whole thing () to your current user:
```
chown -R user:user directory/
```

and try again:
```
docker-compose up
```

* development server will run at `http://localhost:8000/`

## 4
* creating an application with django
```
docker-compose run web python manage.py startapp my_cool_app
```

* create a `templates` directory inside the my_cool_app directory with an `index.html` file with the code below:
```
{% load static %}
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Docker App</title>
</head>

<body>
  <ul>
    <h1>My Cool App 🐳 !</h1>
    <h2> Coolest App Ever 🌊! </h2>
  </ul>
</body>

</html>
```

* in `views.py` add the code below:
```
# Create your views here.
def index(request):
    return render(request, "index.html")
```

* create a `urls.py` file in the `my_cool_app` direcotry and put the code below:
```
from django.urls import path
from . import views


urlpatterns = [
    path('', views.index),
]
```

## 5
* from djangoProjeExample open `settings.py` and add the application
```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'my_cool_app'
]
```

* from djangoProjeExample open `urls.py` and add the code below:
```
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include("my_cool_app.urls")),
]
```

* update:
```
docker-compose up --build
```

* app will run at `http://localhost:8000/`