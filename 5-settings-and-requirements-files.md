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

- Sometimes we’re working on a large project where different developers need different settings, and sharing the same local.py settings module with teammates won’t do (and keep them in version control as well):
```
// settings/local_nhannguyen.py

from .local import *
```
  
- In Unix, an environment variable that is changed in a script or compiled program will only affect that process and possibly child processes. The parent process and any unrelated processes will not be affected. Similarly, changing or removing a variable's value inside a DOS batch file will change the variable for the duration of COMMAND.COM's existence.

- In Unix, the environment variables are normally initialized during system startup by the system init scripts, and hence inherited by all other processes in the system. Users can, and often do, augment them in the profile script for the command shell they are using. In Microsoft Windows, each environment variable's default value is stored in the Windows registry or set in the AUTOEXEC.BAT file.

- Environment variables are local to the process in which they were set. If two shell processes are spawned and the value of an environment variable is changed in one, that change will not be seen by the other.

- When a child process is created, it inherits all the environment variables and their values from the parent process.

- On Mac and many Linux distributions that use bash for the shell, one can add lines like the following to the end of a .bashrc, .bash_profile, or .profile. When dealing with multiple projects using the same API but with di erent keys, you can also place these at the end of your virtualenv’s bin/postactivate script:

```
export VARIABLE_NAME=VALUE
```

- When you set an environment variable via the commands listed above it will remain in existence within that terminal shell until it is unset or the shell is ended. This means that even if you deactivate a virtualenv, the environment variable remains.

```
unset VARIABLE_NAME
```

- If you are using virtualenvwrapper and want to unset environment variables whenever a virtualenv is deactivated, place these commands in the postdeactivate script.

- Normally you should not import ANYTHING from Django directly into your settings, but ImproperlyConfigured is an exception.

- manage.py is automatically created in each Django project. manage.py does the same thing as django-admin but takes care of a few things for you:
  - It puts your project’s package on sys.path.
  - It sets the DJANGO_SETTINGS_MODULE environment variable so that it points to your project’s settings.py file.
  
- Generally, when working on a single Django project, it’s easier to use manage.py than django-admin. If you need to switch between multiple Django settings files, use django-admin with DJANGO_SETTINGS_MODULE or the --settings command line option.

- It’s good practice for each settings file to have its own corresponding requirements file. This means we’re only installing what is required on each server:
  - base.txt
  - local.txt
  - staging.txt
  - production.txt
  _ (ci.txt)
  
```
// requirements/local.txt

-r base.txt  # includes the base.txt requirements file

coverage==4.2
```

- If you want to know how things in your project differ from Django’s defaults, use the diffsettings management command.

- Additional links:
  - [slideshare.net/ jacobian/the-best-and-worst-of-django](https://www.slideshare.net/jacobian/the-best-and-worst-of-django/17-Theeverythingisa_antipattern).
  - [https://en.wikipedia.org/wiki/Environment_variable](https://en.wikipedia.org/wiki/Environment_variable).
  - Packages for Settings Management [github.com/joke2k/django-environ](https://github.com/joke2k/django-environ) (Used in Cookiecutter Django).
  
