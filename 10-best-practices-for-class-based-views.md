- When working with CBVs:
  - Less view code is better.
  - Never repeat code in views.
  - Views should handle presentation logic. Try to keep business logic in models when possible, or in forms if you must.
  - Keep your views simple.
  - Keep you **mixins** simpler.
  
- When using mixins to compose our own view classes, we recommend these rules of inheritance provided by Kenneth Love:
  - The base view classes provided by Django always go to the right.
  - Mixins go to the left of the base view.
  - Mixins should inherit from Python’s built-in object type. Keep your inheritance chain simple!
  
- Which Django GCBV should be used for what task?, Table 10.1, p. 128.

- 3 schools of Django CBV/GCBV usage:
  - Use all the generic views! (we generally belong to the first school)
  - Just use django.views.generic.View
  - Avoid them unless you're actually subclassing views (start with FBV)

- To constrain Django (G)CBV access to authenticated Users:
  - Do not use [decorators in urls.py](https://docs.djangoproject.com/en/1.11/topics/class-based-views/intro/#decorating-class-based-views).
  - Use `LoginRequiredMixin` instead.
  - `LoginRequiredMixin` must always go on the far left side.
  - If you use `LoginRequiredMixin` and override the dispatch method, make sure that the first thing you do is call super dispatch. Any code before the super() call is executed even if the user is not authenticated.
 
- When you need to perform a custom action on a view with a valid form, the **form_valid()** method is where the GCBV work ow sends the request:

```
def form_valid(self, form):
  # Do custom logic here
  return super(FlavorCreateView, self).form_valid(form)
```

- When you need to perform a custom action on a view with an invalid form, the **form_invalid()** method is where the Django GCBV work ow sends the request. This method should return a django.http.HttpResponse:

```
def form_invalid(self, form):
  # Do custom logic here
  return super(FlavorCreateView, self).form_invalid(form)
```

- If you are using class-based views for rendering content, consider using the view object itself to provide access to properties and methods that can be called by other method and properties. They can also be called from templates:
  - This means we can access properties from view object like `view.foo`.
  - Example 10.5, 10.6, p. 133.
  
- Please take notice that the FlavorActionMixin inherits from Python’s object type rather than a pre-existing mixin or view. It’s important that mixins have as shallow inheritance chain as possible. Simplicity is a virtue!

- Additional links:
  - http://ccbv.co.uk/, brilliant!
  - [Method Resolution Order (MRO)](http://python-history.blogspot.com/2010/06/method-resolution-order.html).
  - django-extra-views.
  - django-vanilla-views.
