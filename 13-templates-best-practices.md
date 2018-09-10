- You should structurize your templates as following:
```
templates/
|___base.html
|___... (other sitewide templates in here)
|___some_app/
    |___ ("some_app" templates in here)
```
 
- Some people put app's templates in app's module, that's alright.

- 2-Tier template architecture: all templates inherit from a single root base.html file.
  - This is best for sites with a consistent overall layout from app to app.
```
templates/
|___base.html
|___dashboard.html  # extends base.html
|___profiles/
    |___profile_detail.html # extends base.html
    |___profile_form.html # extends base.html
```
  
- 3-Tier template architecture:
  - Each app has a `base_<app_name>.html` template. App-level base templates share a common parent base.html template.
  - Templates within apps share a common parent `base_<app_name>.html` template.
  - Any template at the same level as base.html inherits base.html.
  - The 3-tier architecture is best for websites where each section requires a distinctive layout.
```
templates/
|___base.html
|___dashboard.html  # extends base.html
|___profiles/
    |___base_profiles.html  # extends base.html
    |___profile_detail.html # extends base_profiles.html
    |___profile_form.html # extends base_profiles.html
```

- When you have large, multi-line chunks of the same or very similar code in separate templates, refactoring that code into reusable blocks will make your code more maintainable.

- Whenever you iterate over a queryset in a template, ask yourself the following questions:
  - How large is the queryset? Looping over gigantic querysets in your templates is almost always a bad idea.
  - How large are the objects being retrieved? Are all the fields needed in this template?
  - During each iteration of the loop, how much processing occurs?
  
- Gotcha in Django Template, suppose you have:
```
class User(models.Model):
  cart = models.ForeignKey(Cart,..)
  
class Cart(models.Model)
  ...
  
# In template you call this:
{% for user in User %}
# DON'T DO THIS: Generated implicit query per user!
{{ user.cart... }}
{% endfor %}

Solution is in view, you return User.objects.all().select_related('cart') insteach of just User.objects.all(). Short explanation:

-------------NO select_relate()-------------
# Hit the db: SELECT * FROM User WHERE id = 1;
user = User.objects.get(id=1)

# Hit the db again: SELECT * FROM Cart, User WHERE User.cart = cart;
cart = user.cart

# FYI: later accesses to `user.cart` will not hit the db
cart = user.cart  # Get the result that is cached when called the first time
---------------------------------------------

---------------select_relate()-------------
# Hit the db: Doing some SQL join
user = User.objects.get(id=1).select_related('cart')

# Doesn't hit the db
cart = user.cart
---------------------------------------------

So the idea is doing a single more complex query but means later use of foreign-key relationships won’t require database queries.
```

- Projects that handle a lot of image or data processing increase the performance of their site by taking the image processing out of templates and into views, models, helper methods, or asynchronous messages queues like Celery or Django Channels.

- `{{ block.super }}`
```
# base.html
{% block content %}
<h1>Base</h1>
{% endblock %}

# child.html
{% extends "base.html" %}
{% block content %}
{{ block.super }}  # To display content of parent block
<h2>Child</h2>
{% endblock %}
```

- We include the name of the block tag in the endblock. Never write just {% endblock %}, include the whole {% endblock javascript %}.

- Templates called by other templates are prefixed with ‘_’.  is applies to templates called via {% include %} or custom template tags. It does not apply to templates inheritance controls such as {% extends %} or {% block %}.

- We suggest serving your error pages from a static file server (e.g. Nginx or Apache) as entirely self- contained static HTML files. That way, if your entire Django site goes down but your static file server is still up, then your error pages can still be served.
- Avoid Coupling Styles Too Tightly to Python Code:
  - Use CSS for styling whenever possible. Never hardcode things like menu bar widths and color choices into your Python code. Avoid even putting that type of styling into your Django template.
  - If you have magic constants in your Python code that are entirely related to visual design layout, you should probably move them to a CSS file.
  - The same applies to JavaScript.

- Additional resources:
  - [Django template language](https://docs.djangoproject.com/en/2.0/topics/templates/#module-django.template)
  - [select_related()](https://docs.djangoproject.com/en/2.0/ref/models/querysets/#select-related)
