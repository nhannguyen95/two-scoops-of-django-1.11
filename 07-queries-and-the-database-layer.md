- In views such as detail pages where you want to retrieve a single object and do something with it, use get_object_or_404() instead of get():
  - Only use it in views.
  - Don’t use it in helper functions, forms, model methods or anything that is not a view or directly view related.
  
- ObjectDoesNotExist can be applied to any model object, whereas Model.DoesNotExist is for a specific model.

- [**QuerySets are lazy**](https://docs.djangoproject.com/en/1.11/topics/db/queries/#querysets-are-lazy) – the act of creating a QuerySet doesn’t involve any database activity. You can stack filters together all day long, and Django won’t actually run the query until the QuerySet is evaluated: [**When QuerySets are evaluated**](https://docs.djangoproject.com/en/2.1/ref/models/querysets/#when-querysets-are-evaluated):
  - **Iteration**: `for e in Entry.objects.all()`
  - **Slicing** (if you use the "step" parameter)
  - **Pickling/Caching**
  - **repr()**
  - **len()**: much more efficient to use Count()
  - **list()**: `entry_list = list(Entry.objects.all())`
  - **book()**: Testing a QuerySet in a boolean context, such as using book(), or, and, or and if statement: `if Entry.objects.filter(..)`

- With complex queries, attempt to avoid chaining too much functionality on a small set of lines. Instead of being forced to chain many methods and advanced database features on a single line, we can break them up over as many lines as needed. This increases readability, which improves the ease of maintenance.

- Django’s ORM is easy to learn, intuitive, and covers many use cases. Yet there are a number of things it does not do well. **What happens then is after the queryset is returned we begin processing more and more data in Python. This is a shame, because every database manages and transforms data faster than Python** (or Ruby, JavaScript, Go, Java, et al).
Instead of managing data with Python, we always try to use Django’s advanced query tools to do the lifting (getting the database, rather than Python, to do work). In doing so we not only benefit from increased performance, we also enjoy using code that is more proven (Django and most databases are constantly tested) than any Python-based workarounds we create.
  - [Query Expressions](https://docs.djangoproject.com/en/1.11/ref/models/expressions/).
  - [Database Functions](https://docs.djangoproject.com/en/1.11/ref/models/database-functions/).

- Don’t Drop Down to Raw SQL Until It’s Necessary:
  - Whenever we write raw SQL we lose elements of security and reusability.
  - If expressing your query as raw SQL would drastically simplify your Python code or the SQL generated by the ORM, then go ahead and do it. For example, if you’re chaining a number of QuerySet operations that each operate on a large data set, there may be a more efficient way to write it as raw SQL.

- Add Indexes as Needed?

- Transactions?
  - When writing views that create/update/delete records but interact with non-database items, you may choose to decorate the view with transaction.non_atomic_requests(). ( “This decorator requires tight coupling between views and models, which will make a code base harder to maintain. We might have come up with a better design if we hadn’t had to provide for backwards-compatibility.”)
  

- Additional links:
  - [Race Condition](https://stackoverflow.com/questions/34510/what-is-a-race-condition).
  - [What does “atomic” mean in programming?](https://stackoverflow.com/questions/15054086/what-does-atomic-mean-in-programming).
