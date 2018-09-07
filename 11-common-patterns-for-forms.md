#### Five Common Form Patterns

- **Pattern 1**: Simple ModelForm with default validators

  - CustomCreateView is assigned X as its model.
  - The view auto-generates a ModelForm based on the X model.
  - This ModelForm relies on the default field validation rules of the X model.
  
```
class CustomCreateView(CreateView):
  model = X
  fields = [Y]
```
  
- **Pattern 2**: **Custom Form Field Validators** in ModelForms

  - In Django, a custom field validator is simply a callable (usually a function) that raises an error if the submitted argument doesn’t pass its test.
  - Since validators are critical in keeping corruption out of Django project databases, it’s especially important to write detailed tests for them.

```
# In validators.py
from django.core.exceptions import ValidationError
def validate_something(value):
  if invalide:
    msg = '..'
    raise ValidationError(msg)
    
# Use in models
class SomeModel(models.Model):
  title = models.CharField(max_length=255, validators=[validate_something])
  
# Use in forms
class SomeForm(forms.ModelForm):
  def __init__(self, *args, **kwargs):
    super(SomeForm, self).__init__(*args, **kwargs)
    self.fields['title'].validators.append(validate_something)
  class Meta:
    model = MyModel
```

- **Pattern 3**: Overriding the Clean Stage of Validation

  - After the default and custom field validators are run, Django provides a _second stage_ and process for validating incoming data, this time via the **clean()** method and **clean_<field_name>()** methods, why?
    - The clean() method is the place to validate two or more fields against each other, since it’s not specific to any one particular field.
    - Validation involving existing data from the database that has already been validated.
    
  - **Important: you can retrieve database inside these clean methods**.
  
```
def clean_some_field(self):
  some_field = self.cleaned_data['some_field']
  # Validate..
  return some_field
  
def clean(self):
  cleaned_data = super(MyForm, self).clean()
  # Validate..
  return cleaned_data
  
# Raise a ValidationError
from django import forms
msg = ''
raise forms.ValidationError(msg)
```

- **Pattern 4**: Hacking Form Fields (2CBVs, 2 Forms, 1 Model)
  
  - Problem: Have a Model, how to custom fields in its corresponsing ModelForm?
  - Solution: instantiated (Django) form objects store fields in a dict-like attribute called `fields`, we can modify existing attributes on specified fields in the `__init__()` method of the ModelForm.

```
class MyModel(models.Model):
  title = models.CharField(max_length=255)  # required
  description = models.CharField(max_length=255, blank=True)  # not required
  
class MyModelForm(forms.ModelForm):
  class Meta:
    model = MyModel
    
  def __init__(self, *args, **kwargs):
    # Call the original __init__ method before assigning field overloads
    super(MyModelForm, self).__init__(*args, **kwargs)
    self.fields['description'].required = True  # customization

# Another form can inherit from MyModelForm (1 Model, many Forms, many CBVs)

# In CBV
class MyModelView(CreateView):
  model = MyModel
  form_class = MyModelForm
```
  
- **Pattern 5**: Reusable Search Mixin View

  - Problem: 2 views with 2 corresponding models, 2 models have `title`, how to reuse a search form?
  - Solution: Using mixin, let 2 views inheret from it.
  - Mixins are a good way to reuse code, but using too many mixins in a single class makes for very hard-to-maintain code. As always, try to keep your code as simple as possible.
  
```
class TitleSearchMixin:
  def get_queryset(self):
    # Fetch the queryset from the parent's get_queryset
    queryset = super(TitleSearchMixin, self).get_query_set()
    
    # Get the q GET parameter
    q = self.request.GET.get('q')
    if q:
      # Return a filtered queryset
      return queryset.filter(title__icontains=q)
    return queryset
    
# Inheret in views that need title search functionality
class SomeListView(TitleSearchMixin, ListView):
  model = MyModel
  
# In template
<form action="" method="GET">
  <input type="text" name="q" />
  <button type="submit">search</button>
</form>
```
  
---

- Additional links:
  - django-floppyforms for rendering Django inputs in HTML5.
  - django-crispy-forms for advanced form layout controls. By default, forms are rendered with Twitter Bootstrap form elements and styles. This package plays well with django-floppyforms, so they are often used together.
  - django-forms-bootstrap is a simple tool for rendering Django forms using Twitter Bootstrap styles. This package plays well with django- oppyforms but con icts with django-crispy-forms.

