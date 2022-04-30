---
layout: post
title: How to create web site configuration model?
categories: [Python, Django]
author_github: negativename
author: Vladislav Kobenko
---

This article describes how to create a class with a single instance. For example, In case you want to create web site configuration model, that would hold application-wide customization like URL or logo.

## How to create SingletoneModel?

We will use `django-solo` package. First download it:

```
    pip3 install django-solo
```

Now you need to include this package into your `settings.py` file:

```
    INSTALLED_APPS = [
        'django.contrib.admin',
        'django.contrib.auth',
        'django.contrib.contenttypes',
        'django.contrib.sessions',
        'django.contrib.messages',
        'django.contrib.staticfiles',
        'pages',
        'solo'
    ]
```

## How to use SingletoneModel?

First you need to include `django-solo` package into your model file:

```python
    from solo.models import SingletonModel
```

Now we create model class `WebSiteConfig` and add some fields for your site configuration:

```python
    class MWebSiteConfig(SingletonModel):
        description = models.TextField("Site description", max_length=512, null=False, blank=False, unique=False, default="some description")
        address = models.TextField("Address", max_length=512, null=True, blank=True, unique=False)
        email = models.EmailField("Email", null=True, blank=True, unique=False)
```

## How to add SingletoneModel into your admin panel?

Include `django-solo` package into your admin file:

```python
    from solo.admin import SingletonModelAdmin
```

Then create a class for administration your site config and register this class:

```python
    from .models import WebSiteConfig

    class WebSiteConfigAdmin(SingletonModelAdmin):
        list_display = ('description', 'address', 'email',)

    # another elements here#

    admin.site.register(WebSiteConfig, WebSiteConfigAdmin)
```

It would be nice if we could keep our code a bit more concise.