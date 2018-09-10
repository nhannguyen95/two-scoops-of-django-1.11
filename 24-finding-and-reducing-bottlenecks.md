Forewords: You should really dig into this chapter in the book, there are a lot of cool stuff that cannot be fully covered in this note.

---

- [Understand QuerySet](https://github.com/nhannguyen95/two-scoops-of-django-1.11/blob/master/07-queries-and-the-database-layer.md):
  - QuerySets are lazy
  - When thay are evaluated
  - how the data is held in memory
 
- Each QuerySet contains a cache to minimize database access: In a newly created QuerySet, the cache is empty. The first time a QuerySet is evaluated – and, hence, a database query happens – Django saves the query results in the QuerySet’s cache and returns the results that have been explicitly requested (e.g., the next element, if the QuerySet is being iterated over). Subsequent evaluations of the QuerySet reuse the cached results.

```
print([e.headline for e in Entry.objects.all()])
print([e.pub_date for e in Entry.objects.all()])
Two problems with this code (if we want the consistency of the `e` object in 2 lines of code):
  - The same db will be executed twice.
  - Entry may be modified between 2 requests.
  
=> Solution: save the QuerySet and reuse it:
queryset = Entry.objects.all()
print([p.headline for p in queryset])  # Execute SQL, save result in cache
print([p.pub_date for p in queryset])  # Reuse the result from cache
```

- However, [limiting the queryset](https://docs.djangoproject.com/en/2.0/topics/db/queries/#limiting-querysets) using an array slice or an index will not populate the cache:

```
queryset = Entry.objects.all()
print(queryset[5]) # Queries the database
print(queryset[5]) # Queries the database again
```

- Some examples of other actions that will result in the entire queryset being evaluated and therefore populate the cache:

- Identify Specific Places to Cache:
  - Which views/templates contain the most queries?
  - Which URLs are being requested the most?
  - When should a cache for a page be invalidated?
  
- Upstream caches such as Varnish are very useful. They run in front of your web server and speed up web page or content serving significantly. See varnish-cache.org.

- Using a CDN (Content Delivery Network) rather than serving static content from your application servers can speed up your projects.

```
[entry for entry in queryset]
bool(queryset)
entry in queryset
list(queryset)

# Then:
queryset[0]  # Use cache
```

- Simply printing the queryset will not populate the cache. This is because the call to __repr__() only returns a slice of the entire queryset.

---

- As well as caching of the whole QuerySet, there is caching of the result of attributes on ORM objects. In general, attributes that are not callable will be cached, but callable attributes cause DB lookups every time:

```
# Attributes that are not callable
entry = Entry.objects.get(id=1)
entry.blog   # Blog object is retrieved at this point
entry.blog   # cached version, no DB access

# Callable attributes
entry.authors.all()   # query performed
entry.authors.all()   # query performed again
```

---
  
- Try using [select_related()](https://github.com/nhannguyen95/two-scoops-of-django-1.11/blob/master/13-templates-best-practices.md) in your ORM calls to combine queries.

- For many-to-many and many-to-one relationships that can’t be optimized with select_related(), explore using prefetch_related() instead.
  
- Additional resources:
  - [Database access optimization](https://docs.djangoproject.com/en/1.11/topics/db/optimization/), must read!
  - django-debug-toolbar: help determine where most of your queries are coming from.
  - django-cache-panel: only run at local, this increases visibility into what your cache is doing.
  - django-extensions
  - [silk](https://github.com/jazzband/django-silk)
  - To understand cache:
    - [How Caching Works](https://computer.howstuffworks.com/cache3.htm)
    - [what is the difference between cpu cache and memory cache](https://stackoverflow.com/questions/42435180/what-is-the-difference-between-cpu-cache-and-memory-cache)
    - [Software Cache](http://wiki.c2.com/?SoftwareCache)
  - [Django’s cache framework](https://docs.djangoproject.com/en/1.11/topics/cache/)
  - [highperformancedjango.com](https://highperformancedjango.com/)
