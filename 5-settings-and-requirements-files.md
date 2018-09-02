- Django 1.11 has over 150 settings that can be controlled in the settings module, most of which come with default values. Settings are loaded when your server starts up, and experienced Django developers stay away from trying to change settings in production since they require a server restart.

- The SECRET_KEY setting is used in Django’s cryptographic signing functionality.

- [A common 4-tier architecture](https://en.wikipedia.org/wiki/Deployment_environment):
  - Local (development).
  - Staging.
  - Test.
  - Production.
  
- Instead of having one settings.py file, with this setup you have a settings/ directory containing your settings files.  is directory will typically contain something like the following:
  - __init__.py
  - base.py
  - local.py: is is the settings file that you use when you’re working on the project locally. Local development-specific settings include DEBUG mode, log level, and activation of developer tools like django-debug-toolbar.
  - staging.py: Staging version for running a semi-private version of the site on a production server. This is is where managers and clients should be looking before your work is moved to production.
  - test.py: Settings for running tests including test runners, in-memory database definitions, and log settings.
  - production.py:  is is the settings file used by your live production server(s). That is, the server(s) that host the real live website. This file contains production-level settings only. It is sometimes called prod.py.
  
- You’ll also want to have a ci.py module containing that server’s settings (Continuous Integration Servers).

- Each settings module should have its own corresponding requirements file.

- Let’s take a look at how to use the shell and runserver management commands with this setup:
  - python manage.py shell --settings=twoscoops.settings.local
  - python manage.py runserver --settings=twoscoops.settings.local
  
- A great alternative to using the --settings command line option everywhere is to set the DJANGO_SETTINGS_MODULE and PYTHONPATH environment variable to your desired settings module path. You’d have to set DJANGO_SETTINGS_MODULE to the corresponding settings module for each environment, of course.

- For those with a more comprehensive understanding of virtualenvwrapper, another alternative is to set DJANGO_SETTINGS_MODULE and PYTHONPATH in the postactivate script and unset them in the postdeactivate script. Then, once the virtualenv is activated, you can just type python from anywhere and import those values into your project. This also means that typing django-admin.py at the command-line works without the -- settings option.

- Sometimes we’re working on a large project where different developers need different settings, and sharing the same local.py settings module with teammates won’t do:
```
settings/local_nhannguyen.py

from .local import *
```
  
- Additional links:
  - [slideshare.net/ jacobian/the-best-and-worst-of-django](https://www.slideshare.net/jacobian/the-best-and-worst-of-django/17-Theeverythingisa_antipattern).
