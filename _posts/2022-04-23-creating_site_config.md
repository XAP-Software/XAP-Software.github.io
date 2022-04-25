---
layout: post
title: How to create web site configuration model?
categories: [Python, Django]
author_github: negativename
author: Kobenko Vladislav
---

In case you need to create web site configuration model, which means, that you can only create one instance of this class.

## How to create SingletoneModel?

First of all you need to download `django-solo` package:

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

In the start you need to include `django-solo` package into your model file:

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

First of all you need to include `django-solo` package into your admin file:

```python
    from solo.admin import SingletonModelAdmin
```

Then create class for administration your site config and register this class:

```python
    from .models import WebSiteConfig

    class WebSiteConfigAdmin(SingletonModelAdmin):
        list_display = ('description', 'address', 'email',)

    # another elements here#

    admin.site.register(WebSiteConfig, WebSiteConfigAdmin)
```

It would be nice if we could keep our code a bit more concise.