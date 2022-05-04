---
layout: post
title: How to change elements position from UI in django-admin?
categories: [Python, Django]
author_github: nevermore17
author: Verbin Kirill
---

In case you need to change position of elements within django admin, you can use this article to understand how.

## How to add django-admin-sortable2 package?

We will use `django-admin-sortable2` package. First download it:

```
    pip3 install django-admin-sortable2
```

Now you need to include this package into your `settings.py` file:

```python
    INSTALLED_APPS = [
        'django.contrib.admin',
        'django.contrib.auth',
        'django.contrib.contenttypes',
        'django.contrib.sessions',
        'django.contrib.messages',
        'django.contrib.staticfiles',
        '...',
        'adminsortable2'
    ]
```
## Creating order field

In `application/models.py` add ordering field. Any valid Python variable name will work. This value be specified as the primary field used for sorting.

```python

    class MyModel(models.Model):
        
        ...

        my_ordering = models.PositiveIntegerField(
            default=0,
            blank=False,
            null=False,
        )

        class Meta:
            ordering = ['my_ordering']

```

Use one of these models fields for ordering field:

- BigIntegerField
- IntegerField
- PositiveIntegerField 
- PositiveSmallIntegerField
- SmallIntegerField

## In application/admin.py

In `application/admin.py` create `MyModelAdmin` class as SortableAdminMixin and admin.ModelAdmin child


```python
from django.contrib import admin
from adminsortable2.admin import SortableAdminMixin

from application.models import MyModel


@admin.register(MyModel)
class MyModelAdmin(SortableAdminMixin, admin.ModelAdmin):
    pass
```

