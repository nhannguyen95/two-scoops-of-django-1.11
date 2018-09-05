- Whenever you implement a view, think about whether it would make more sense to implement as a FBV or as a CBV: Figure 8.1, p. 104.

- Keep View Logic Out of URLConfs (the coupling of views with urls is loose, allows for in nite  exibility, and encourages best practices).

- Try to Keep Business Logic Out of Views:
  - whenever we find ourselves duplicating business logic instead of Django boilerplate between views, it’s time to move code out of the view.

- Django Views Are Functions:

```
HttpResponse = view(HttpRequest)
```

- Additional links:
  - [URL design](https://docs.djangoproject.com/en/1.11/misc/design-philosophies/#url-design).
  
