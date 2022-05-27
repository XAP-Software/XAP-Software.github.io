---
layout: post
title: How to send emails using Django and Postfix?
categories: [Python, Django]
author_github: negativename
author: Kobenko Vladislav
---

In case you need to add a possibility of sending email notifications from your `Django` app and use `Postfix` mail transfer agent then this article will help you out.

# Configure settings.py file

Firstly, you need to add following lines in `settings.py` file. You should insert host address in `EMAIL_HOST` variable and insert port in `EMAIL_PORT`. Notice, that the port of your local `Postfix` service is `25` by default.

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

For example, if you have a call-back form on the front-end and you send user's data to your `Django API` on the back-end, you should rewrite the default `create()` method of `View class`. In order to show messages in `Django` admin panel you need to create an object of your `Form` class and save it using default function `save()`.

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

Now you can see that using `create()` function you receive data from the front-end and create a message using this data. Also, you can fill in `from_addr` field with address which will be used by `Postfix` to send email. The field `recipient_list` is used for specifying recipients of your email. As a result, you send messages using function `send_mail()` in accordance with these params.

Note that you need to set up `Postfix` configuration before using the method above.
