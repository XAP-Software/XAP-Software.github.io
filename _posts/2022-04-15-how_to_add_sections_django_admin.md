---
layout: post
title: How to add/change sections, elements and properties for Django admin?
categories: [Python, Django]
author_github: negativename
author: Vladislav Kobenko
---

In case you need to use create or change sections, elements and their propeties in Django admin.

## How to add or change sections in Django Admin?

In file `apps.py` in your configuration class you can add `verbose_name`:

```python
    class ContentConfig(AppConfig):
        default_auto_field = 'django.db.models.BigAutoField'
        name = 'content'
        verbose_name = "Content"
```

Now your section name for this app is `Content`.

## How to add or change elements for your section?

In Django framework elements for sections are models, which you define in `models.py` file. So to add your element in Django admin panel you need to create model:

```python
    class Service(models.Model):

        class Meta:
            verbose_name = "Service"
            verbose_name_plural = "Services"

        title = models.CharField("Name",max_length=200)        
        site_title = models.CharField("Title", max_length=100,  null=True, blank=True)
        site_hru = models.CharField("Human readable URL", max_length=100, null=True, blank=True)
        site_h1 = models.CharField("Page header(h1)", max_length=100,  null=True, blank=True)
```

Now you have section's element with properties title and image. In Django admin panel in section `Content` you will see `Services`. If you change fields `verbose_name` and `verbose_name_plural` your element's name will change after that.

## How to add or change properties of elements that you want to see in Django admin panel?

We need to change `admin.py` file:

```python
    from .models import Service

    class ServicesAdmin(admin.ModelAdmin):
        list_display = ("title", "site_title", "site_hru")

    # another elements here#

    admin.site.register(Service, ServicesAdmin)
```
First of all we need to import our model for `models.py` file. Now create an Admin class for this model, in field `list_display` we are choosing the properties, which user will see in Django admin panel.
After that we need to register it to show this element and properties in Django admin panel.

It would be nice if we could keep our code a bit more concise.
