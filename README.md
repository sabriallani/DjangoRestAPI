
# Django Rest Framework Basic CRUD Example

This is a simple tutorial to help you create a basic Django Rest Framework API for a Todo list application.

## 1. Installation

Make sure to install Django and Django Rest Framework:

```bash
pip install django djangorestframework
```

## 2. Create a Django Project

Create a new Django project:

```bash
django-admin startproject myproject
cd myproject
```

## 3. Create a Todo App

Inside the project directory, create a new application called `todo`:

```bash
python manage.py startapp todo
```

## 4. Add the App and Rest Framework to INSTALLED_APPS

In `myproject/settings.py`, add `rest_framework` and `todo` to the `INSTALLED_APPS`:

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'rest_framework',
    'todo',
]
```

## 5. Define the Task Model

In `todo/models.py`, create a model for tasks:

```python
from django.db import models

class Task(models.Model):
    title = models.CharField(max_length=100)
    description = models.TextField()
    completed = models.BooleanField(default=False)

    def __str__(self):
        return self.title
```

## 6. Create a Serializer for the Task Model

Create a file called `serializers.py` inside the `todo/` directory and define the serializer:

```python
from rest_framework import serializers
from .models import Task

class TaskSerializer(serializers.ModelSerializer):
    class Meta:
        model = Task
        fields = '__all__'
```

## 7. Create API Views for Task

In `todo/views.py`, create views for listing, creating, updating, and deleting tasks:

```python
from rest_framework import generics
from .models import Task
from .serializers import TaskSerializer

class TaskListCreate(generics.ListCreateAPIView):
    queryset = Task.objects.all()
    serializer_class = TaskSerializer

class TaskRetrieveUpdateDestroy(generics.RetrieveUpdateDestroyAPIView):
    queryset = Task.objects.all()
    serializer_class = TaskSerializer
```

## 8. Define URLs for the API

Create a `urls.py` file in the `todo/` directory:

```python
from django.urls import path
from . import views

urlpatterns = [
    path('tasks/', views.TaskListCreate.as_view(), name='task-list-create'),
    path('tasks/<int:pk>/', views.TaskRetrieveUpdateDestroy.as_view(), name='task-detail'),
]
```

Then include the `todo` app's URLs in the project’s main `urls.py` file (`myproject/urls.py`):

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/', include('todo.urls')),
]
```

## 9. Migrate the Database

Run the migrations to create the necessary database tables:

```bash
python manage.py makemigrations
python manage.py migrate
```

## 10. Run the Development Server

Start the Django development server:

```bash
python manage.py runserver
```

## 11. Testing the API

Now you can test the API using Postman or a browser:

- **List all tasks (GET)**: `http://127.0.0.1:8000/api/tasks/`
- **Create a task (POST)**: `http://127.0.0.1:8000/api/tasks/`
- **Get a task by ID (GET)**: `http://127.0.0.1:8000/api/tasks/<id>/`
- **Update a task (PUT/PATCH)**: `http://127.0.0.1:8000/api/tasks/<id>/`
- **Delete a task (DELETE)**: `http://127.0.0.1:8000/api/tasks/<id>/`

### Example JSON for creating a task:

```json
{
    "title": "Learn Django",
    "description": "Go through Django Rest Framework tutorials",
    "completed": false
}
```
