- The most important thing to remember about Django forms is they should be used to validate all incoming data.

  - Problem: have a dict contains field data of a model, want to save them into db, how to validate them before saving?
  - Solution: use a form corresponding to that model, define all kind of validation, then `form = Form(dict)`, if `form.is_valid()` then `form.save()`. Get errors from `form.errors`.
  - What’s really nice about this practice is that rather than cooking up our own validation system for incoming data, we’re using the well-proven data testing framework built into Django.
  
- When you call `form.is_valid()`:
  - `form.is_valid()` calls `form.full_clean()`.
  - `form.full_clean()` iterates through the form fields and each field validates itself:
    - Data coming into the field is coerced into Python via the `to_python()` method or raises a `ValidationError`.
    - Data is validated against field-specific rules, including custom validators. Failure raises a `ValidationError`.
    - If there are any custom `clean_<field>()` methods in the form, they are called at this time.
  - `form.full_clean()` executes the `form.clean()` method.
  - If it's a `ModelForm` instance, form._post_clean() does the following:
    - Sets `ModelForm` data to the `Model` instance, regardless of whether `form.is_valid()` is `True` or `False`.
    - Calls the model's `clean()` method. For reference, saving a model instance through the ORM does not call the model's `clean()` method.

- Can use `form.add_error()` within `form.clean()`.

- Customizing Widgets:
  - As always, keep it simple! Stay focused on presentation, nothing more.
  - No widgets should ever change data. They are meant purely for display.
  - Follow the Django pattern and put all custom widgets into modules called widgets.py.
  
- If you want to know which templates can be overidden, you can look inside Django’s source code at github.com/django/django/tree/master/django/forms/templates/django/forms/widgets.

- If you want to create new custom widgets:
  - Go to https://github.com/django/django/blob/master/django/forms/widgets.py and select the widget closest to what you want.
  - Extend the widget to behave as you would like. Keep changes to a minimum!
  - Notes:
    - All the widget does is modify how the value is displayed.
    - The widget does not validate or modify the data coming back from the browser. That’s the job of forms and models respectively.
    
    

- Additional links:
  - https://docs.djangoproject.com/en/1.11/ref/forms/validation/#raising-validationerror.
  - https://docs.djangoproject.com/en/1.11/ref/csrf/.
  - https://docs.djangoproject.com/en/1.11/ref/forms/api/#django.forms.Form.errors.as_data.
  - https://docs.djangoproject.com/en/1.11/ref/forms/api/#django.forms.Form.errors.as_json.
  - https://docs.djangoproject.com/en/1.11/ref/forms/api/#django.forms.Form.non_field_errors.
  - [Overriding built-in widget templates](https://docs.djangoproject.com/en/1.11/ref/forms/renderers/#overriding-built-in-widget-templates).
  - [TemplatesSetting](https://docs.djangoproject.com/en/1.11/ref/forms/renderers/#templatessetting).
  - https://www.pydanny.com/tag/forms.html
  - [Nice ArrayField widgets with choices and chosen.js](https://bradmontgomery.net/blog/2015/04/25/nice-arrayfield-widgets-choices-and-chosenjs/)
  - [The form rendering API](https://docs.djangoproject.com/en/1.11/ref/forms/renderers/)
  
  
