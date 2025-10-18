# Django-notes

### Project Setup:
---
First we need to setup a django project in a virtual environment so that other packages don't conflict with the packeges used in the project.
The following code creates a python virtual environment:
```cmd
python -m venv env
```
We have to activate the environment by running the following command:
```cmd
.\env\Scripts\activate
```
Now we have to install the required packages with pythons package manager called ***pip***.
```cmd
pip install django
pip install pillow
```
To see what packages we have installed we can use the following command:
```cmd
pip freeze
```
After installing all the necessary packages, we have to start a project by writing the following command:
```cmd
django-admin startproject project_name
```
After running the commands above, we can se a project folder with the given project name. We can open that folder in the code editor and start coding.
We can also setup the project further by running the following commands:
```cmd
cd project_name
python manage.py startapp app_name
mkdir templates
code .
```
`code .` will open the project in vscode. Then we have to configure the settings.py file in the project folder.

1. Add `app_name` to `INSTALLED_APP` list:

 ![Add app_name to INSTALLED_APP list](/images/addInstalledApp.jpg)

 2. Add `templates` folder name to the `TEMPLATES` list:

 ![Add template to TEMPLATES list](/images/addTemplates.jpg)

 3. Configure static and media files by adding these lines at the bottom of `settings.py`:

 ![Add STATIC and MEDIA folder paths](/images/static&media.jpg)

 ## Database Migration and Superuser
---

###Running migrations:

`makemigrations` makes migrations files which contains changes to schema.
`migrate` applies changes from migrations files to database.

 ```cmd
python manage.py makemigrations  

python manage.py migrate
```

### Creating Super User:

```cmd
python manage.py createsuperuser 
```

![Creating Super User](/images/addSuperUser.jpg)

### Running dev server:
The following command runs the project in the development server. We can specify a port number. 
The default port number is `8000`
```cmd
python manage.py runserver 8080
```
### Creating Models:

Models are python sub classes of `models.Model`.

```python
from django.db import models

class Note(models.Model):
    title = models.CharField(max_length=200)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    is_pinned = models.BooleanField(default=False)
    attachment = models.FileField(upload_to='attachments/', null=True, blank=True)
    priority = models.PositiveSmallIntegerField(default=1)

    def __str__(self):
        return self.title
```

### Admin

We have to register the models in `admin.py` file. Here is and example admin page:

```python
from django.contrib import admin
from myapp.models import *

admin.site.register([CustomUser, Note])
```

### Templates:

`base.html` is the layout file which defines the general layout of the website.
Usually this is where navbar, footer and other parts of the website which are common for all the pages are defined.

```HTML
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Website Title</title>
    {% block head %} {% endblock head %}
  </head>
  <body>
    <ul>
      <li><a href="{% url 'doctorPage' %}">Home</a></li>
    </ul>
    {% block body %} {% endblock body %}
  </body>
</html>
```

Other files usually override the `block` tags specified in `base.html`.

 ```HTML
{% extends 'base.html' %} 
{% block head %}
    <styles></styles>
    <scripts></scripts>
{% endblock head %} 
{% block body %}
<form method="POST">
  {% csrf_token %}
  <label for="fname">Name</label>
  <input type="text" id="fname" name="name" />

  <label for="lname">Specialization</label>
  <select id="country" name="specialization">
    <option value="australia">Australia</option>
  </select>

  <label for="lname">Phone</label>
  <input type="text" id="lname" name="phone" />

  <label for="lname">Email</label>
  <input type="text" id="lname" name="email" />

  <label for="country">Department</label>
  <select id="country" name="department">
    <option value="" selected disabled>Select Department</option>

    {% for d in depts %}
    <option value="{{d.id}}">{{d.name}}</option>
    {% endfor %}
  </select>

  <input type="submit" value="Submit" />
</form>

<table>
  <tr>
    <th>Name</th>
    <th>Specialization</th>
    <th>Phone</th>
    <th>Email</th>
    <th>Department</th>
    <th>Actions</th>
  </tr>

  {% for i in doctors %}
  <tr>
    <td>{{i.name}}</td>
    <td>{{i.specialization}}</td>
    <td>{{i.phone}}</td>
    <td>{{i.email}}</td>
    <td>{{i.department}}</td>
    <td>
      <a href="#">Edit</a>
      <a href="#">Delete</a>
    </td>
  </tr>
  {% endfor %}
</table>

{% endblock body %}
```
Note: To use the `base.html` file as layout, other HTML files must have this line as the first line:
```html
{% extends 'base.html' %}
```

### Views

Views are python functions which handle the logical side of the website and makes the page more dynamic.

Here is an example view function:
```python
from django.shortcuts import render, redirect
from .models import Note

def home(request):
    notes = Note.objects.all()
    return render(request, 'home.html', {'notes': notes})
```

### Adding URLs

URLs are how we access a webpage.
In django we have to configure the `urls.py` file. Here is an example `urls.py` file.

```python
from django.contrib import admin
from django.urls import path, include
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_name.urls')),
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```
