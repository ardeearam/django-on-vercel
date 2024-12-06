# Django 5 + Vercel

This example shows how to use Django 5 on Vercel with Serverless Functions using the [Python Runtime](https://vercel.com/docs/concepts/functions/serverless-functions/runtimes/python).

The purpose of this example is to serve as a Django 5 codebase that deploys to Vercel out-of-the-box. No fuss, no mess.

Feel free to extend this codebase on your own fork by adding your own business logic in your own apps.

## Demo

https://django-on-vercel-beh8.vercel.app/

## How it Works

Our Django application, `example` is configured as an installed application in `api/settings.py`:

```python
# api/settings.py
INSTALLED_APPS = [
    # ...
    'example',
]
```

We allow "\*.vercel.app" subdomains in `ALLOWED_HOSTS`, in addition to 127.0.0.1:

```python
# api/settings.py
ALLOWED_HOSTS = ['127.0.0.1', '.vercel.app']
```

The `wsgi` module must use a public variable named `app` to expose the WSGI application:

```python
# api/wsgi.py
app = get_wsgi_application()
```

The corresponding `WSGI_APPLICATION` setting is configured to use the `app` variable from the `api.wsgi` module:

```python
# api/settings.py
WSGI_APPLICATION = 'api.wsgi.app'
```

There is a single view in `example/views.py` which renders the Django rocketship template in `example/templates/example/index.html` we all know and love:

```python
# example/views.py
from datetime import datetime

from django.http import HttpResponse
from django.shortcuts import render

def index(request):
  now = datetime.now()
  return render(request, 'example/index.html')
```

This view is exposed a URL through `example/urls.py`:

```python
# example/urls.py
from django.urls import path

from example.views import index


urlpatterns = [
    path('', index),
]
```

Finally, it's made accessible to the Django server inside `api/urls.py`:

```python
# api/urls.py
from django.urls import path, include

urlpatterns = [
    ...
    path('', include('example.urls')),
]
```

This example uses the Web Server Gateway Interface (WSGI) with Django to enable handling requests on Vercel with Serverless Functions.

## Running Locally

```bash
python manage.py runserver
```

Your Django application is now available at `http://localhost:8000`.

## Deploying on Vercel

For more instructions on deployment to Vercel, visit: https://ardeearam.dev/posts/where-do-i-host-my-django5-projects-for-free/

