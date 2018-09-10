- The official preferred way to attach ForeignKey, OneToOneField, or ManyToManyField to User is as follows:
```
from django.conf import settings
models.OneToOneField(settings.AUTH_USER_MODEL)
```

- And don't use _get_user_model()_ for Foreign Keys to User, it's bad as it tends to create import loop.

- Additional links:
  - [Django using get_user_model vs settings.AUTH_USER_MODEL](https://stackoverflow.com/questions/24629705/django-using-get-user-model-vs-settings-auth-user-model)
  - [Referencing the User model](https://docs.djangoproject.com/en/2.0/topics/auth/customizing/#referencing-the-user-model)
  - django-authtools is a library that makes defining custom user models easier. Of particular use are the AbstractEmailUser and AbstractNamedUser models. Even if you don’t end up using django-authtools, the source code is well worth examining.
  - [How to Extend Django User Model](https://simpleisbetterthancomplex.com/tutorial/2016/07/22/how-to-extend-django-user-model.html)
