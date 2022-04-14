---
layout: post
title: How to create a django backend server using django-rest-framework?
categories: [Python, Django]
author_github: nevermore17
author: Verbin Kirill
---

In case you need to use django to create only a backend server to use other front-end frameworks (next, nuxt) suggest django-rest-framework to create a rest-api.

## What is django-rest-framework?

`django-rest-framework` is a powerful and flexible toolkit for building Web APIs. Django provides the opportunity to create custom api but `django-rest-framework` has a set of tools to create flexible api in a short period of time with access control, filters and parameters.

## rest_framework.serializers and rest_framework.views

`rest_framework.serializers` includes some Abstruct class (`Serializer`, `ModelSerializer`) for serializing and deserializing the `user_models` instances into representations such as `json`.

`rest_framewort.views` includes `APIView` for create custom rest methods for client.

### serializers.Serializer

Abstruct class is to provide a way of serializing and deserializing the `json` data to django models.

```python
#In myapp/serializers.py
...
from rest_framework import serializers
...

class MySerializer(serializers.Serializer):
    #API description
    id = serializers.IntegerField(read_only=True)
    title = serializers.CharField(required=False, allow_blank=True, max_length=100)
    ...


    def create(self, validated_data):
        ...

    def update(setf, instance, validated_data):
        ...
        instance.title = validated_data.get('title', instance.title)
        ...
        instance.save()
        return instance

```

### views.APIView

`APIView` based class designed to project data into the API.

```python
#In myapp/views.py
from rest_framework import views
from .models import user_model
from .serializers import MySerializer
...

class MyAPIView(views.APIView):
    def get(self, request)
        obj_list = user_model.objects.all()
        serializer = MySerializer(obj_list)
        return(serializer.data)

```

In the end we need to create routes for our `APIView`
```python
#In myapp/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('get/', views.MyAPIView.as_view()),
]
```
```python
#In myproject/urls.py

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path("myapp/", include("myapp.urls")),
]
```

### serializers.ModelSerializer

In case you need to serializing and deserializing simple model rather to use `ModelSerializer` class. It's replicating a lot of information that's also contained in the `user_model` model.
```python
#In myapp/serializers.py
...
from rest_framework import serializers
...

class MyModelSerializer(serializers.ModelSerializer):
    class Meta:
        model = user_model
        fields = ['field1', 'field2', 'field3', ...]
```

### generics.[SomeAPIView]

`rest_framework.generics` provides a set of already mixed-in generic views that we can use to trim down our views.py module even more. 

- `CreateAPIView` - creating data on `POST` request

- `ListAPIView` - read data on `GET` request

- `RetrieveAPIView` - read record on `GET` request

- `DestroyAPIView` - delete data(record) on `DELETE` request

- `UpdateAPIView` - change record on `POST` or `PATCH` request

- `ListCreateAPIView` - read on `GET` request and create list of data on `POST` request

- `RetrieveUpdateAPIView` - read record on `GET` request and change record on `PATCH` request

- `RetrieveDestroyAPIView` - read record on `GET` request and delete record on `DELETE` request

- `RetrieveUpdateDestroyAPIView` - read, change and delete record on `GET`, `PATCH` and `DELETE` request

```python
#In myapp/views.py
from rest_framework import generics
from .models import user_model
from .serializers import MyModelSerializer

class MyListAPIView(generics.ListAPIView):
    queryset = user_model.objects.all()
    serializer_class = MyModelSerializer

```
```python
#In myapp/urls.py

from django.urls import path
from . import views

urlpatterns = [
    path('get/', views.MyListAPIView.as_view()),
]

```

It would be nice if we could keep our code a bit more concise.
