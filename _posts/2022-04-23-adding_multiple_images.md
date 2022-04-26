---
layout: post
title: How to add multiple images in your Django admin panel?
categories: [Python, Django]
author_github: negativename
author: Kobenko Vladislav
---

In case you need to add multiple images in your django admin panel, you can use this article to solve this problem.

## Creating new class for images storing

Django admin does not have embedded functions for adding multiple images, that's why we need to create new class for storing images:

```python
class ProjectGallery(models.Model):
    class Meta:
        verbose_name = "Project image"
        verbose_name_plural = "Project images"

    project = models.ForeignKey('Project', on_delete=models.SET_NULL, null=True, blank=True, verbose_name='Project')
    gallery = models.ImageField('Image', upload_to ='./projects_gallery/', null=True, blank=True)
    
```

This class has two fields. One of them is a foreign key to the parent class and the other field is needed for storing images.

## Registering changes in admin panel

Now we need to register `ProjectGallery` in `admin.py` file:

```python
from .models import Project, ProjectGallery

class ProjectGalleryAdmin(admin.StackedInline):
    model = ProjectGallery
    extra = 0

class ProjectAdmin(admin.ModelAdmin):
    list_display = ("title", "page", "site_hru", "is_published",)
    inlines = [ProjectGalleryAdmin]

admin.site.register(Project, ProjectAdmin)
```

In these classes we used `admin.StackedInline` and `inlines`. This allows user to add images from parent class `Project`.

It would be nice if we could keep our code a bit more concise.