---
layout: post
title: How to send emails using Django and Postfix?
categories: [Python, Django]
author_github: negativename
author: Kobenko Vladislav
---

In case you need to add capabality for sending email notifications from your `Django` app and use `Postfix` mail transfer agent, this article will help you.

# Configure settings.py file

First you need to add following lines in `settings.py` file. In `EMAIL_HOST` variable you should insert host address and in `EMAIL_PORT` insert port. By default port of your local `Postfix` service is `25`.

```python

EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
EMAIL_HOST = 'localhost'
EMAIL_PORT = 25
EMAIL_HOST_USER = ''
EMAIL_HOST_PASSWORD = ''
EMAIL_USE_TLS = False
DEFAULT_FROM_EMAIL = '<whatever@example.com>'

```

# Sending emails

Now you need to open your `views.py` file and import `send_mail` function.

```python
from django.core.mail import send_mail
```

For example, if you have a call-back form on front-end and sending user's data to your `Django API` on back-end, you should rewrite the default `create()` method of view class. To show messages in `Django` admin panel you need to create an object of your `Form` class and save it using default function `save()`.

```python
class TestFormViewSet(viewsets.ModelViewSet):
    queryset = TestForm.objects.all()
    serializer_class = TestFormSerializer

    def create(self, request):
        post_data = request.data

        message = "Email by '%s'\Message: '%s'\n" %
                                (post_data['name'], post_data['message'])

        from_addr = "no-reply@test.com"
        recipient_list = ["test@gmail.com"]
        send_mail("Test subject of an email", message, from_addr, recipient_list)


        new_form = TestForm.objects.create(name=post_data["name"], message=post_data["message"])

        new_form.save()

        serializer = TestFormSerializer(new_form)

        return Response(serializer.data)
```

Now you see, that in the `create()` function you getting data from front-end and creating message using this data. Also in field `from_addr` you write the address from which the `Postfix` send an email. Field `recipient_list` using for specifying recipients of your email. And in the end you are sending messages using function `send_mail()` with these params.

Note: You need to set up `Postfix` configuration before using the above method.
