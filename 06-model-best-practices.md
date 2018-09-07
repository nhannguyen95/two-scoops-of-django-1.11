- If there are 20+ models in a single app, think about ways to break it down into smaller apps, as it probably means your app is doing too much. In practice, we like to lower this number to no more than five models per app.

- At all costs, everyone should avoid multi-table inheritance (since each query on a child table requires joins with all parent tables) since it adds both confusion and substantial overhead. Instead of multi-table inheritance, use explicit OneToOneFields and ForeignKeys between models so you can control when joins are traversed.

- Always back up your data before running a migration.

- Before deployment, check that you can rollback migrations!

- Take the time to make sure that no model should contain data already stored in another model.

- Table 6.2: When to Use Null and Blank by Field (p. 74).

- `null=True`: NULL in db, `blank=True`: corresponding form widget to accept empty values. (`CharField` and similars will store empty string if `blank=True`, `null=False`.

- PostgreSQL expert Frank Wiles on the problems with using a database (BinaryField) as a file store:
  - ‘read/write to a DB is always slower than a file system’.
  - ‘your DB backups grow to be huge and more time consuming’.
  - ‘access to the files now requires going through your app (Django) and DB layers’

- Table 6.3: When to Use Null and Blank for Postgres Fields (p. 80).

- Manipulate at "row-level": Model methods, "table-level": custom Manager method.

- There are two reasons you might want to customize a Manager: to add extra Manager methods, and/or to modify the initial QuerySet the Manager returns.

- If you use custom Manager objects, take note that the first Manager Django encounters (in the order in which they’re defined in the model) has a special status. Django interprets the first Manager defined in a class as the “default” Manager, and several parts of Django (including dumpdata) will use that Manager exclusively for that model. As a result, it’s a good idea to be careful in your choice of default manager in order to avoid a situation where overriding get_queryset() results in an inability to retrieve objects you’d like to work with:
  - You can specify a custom default manager using Meta.default_manager_name, OR:
  - objects = models.Manager() should be defined manually above any custom model manager.
  
- If no managers are declared on a model and/or its parents, Django automatically creates the objects manager.

- The default manager on a class is either the one chosen with Meta.default_manager_name, or the first manager declared on the model, or the default manager of the first parent model.

- The concept of fat models is that rather than putting data-related code in views and templates, instead we encapsulate the logic in model methods, classmethods, properties, even manager methods. That way, any view or task can use the same logic.

- The problem with putting all logic into models is it can cause models to explode in size of code, becoming what is called a ‘god object’. This anti-pattern results in model classes that are hundreds, thousands, even tens of thousands of lines of code. Because of their size and complexity, god objects are hard to understand, hence hard to test and maintain:
  - When moving logic into models, we try to remember one of the basic ideas of object-oriented programming, that big problems are easier to resolve when broken up into smaller problems.
  - The methods, classmethods, and properties are kept, but the logic they contain is moved into Model Behaviors (Mixins) or Stateless Helper Functions. Alone neither of these techniques are perfect. However, when both are used judiciously, they can make projects shine. Understanding when to use either isn’t a static science, it is an evolving process. This kind of evolution is tricky, prompting our suggestion to have tests for the components of fat models.

- Stateless Helper Functions: By moving logic out of models and into utility functions, it becomes more isolated. This isolation makes it easier to write tests for the logic. The downside is that the functions are stateless, hence all arguments have to be passed.

- Start normalized, and only denormalize if you’ve already explored other options thoroughly. You may be able to simplify slow, complex queries by dropping down to raw SQL, or you may be able to address your performance issues with caching in the right places.

- Don’t forget to use indexes. Add indexes when you have a better feel for how you’re using data throughout your project.

- You may find django-model-utils and django-extensions pretty handy.

- Additional links:
  - [https://docs.djangoproject.com/en/1.11/topics/db/models/#model-inheritance](https://docs.djangoproject.com/en/1.11/topics/db/models/#model-inheritance) (So goood).
   - [docs.djangoproject.com/en/1.11/ref/migration-operations/#runpython](https://docs.djangoproject.com/en/1.11/ref/migration-operations/#runpython).
   - [docs.djangoproject.com/en/1.11/ref/migration-operations/#runsql](https://docs.djangoproject.com/en/1.11/ref/migration-operations/#runsql).
   - [Relational Database Design/Normalization](https://en.wikibooks.org/wiki/Relational_Database_Design/Normalization).
   - [Three things you should never put in your database](https://www.revsys.com/tidbits/three-things-you-should-never-put-your-database/).
   - [Avoid Django’s GenericForeignKey](https://lukeplant.me.uk/blog/posts/avoid-django-genericforeignkey/).
   - https://docs.djangoproject.com/en/1.11/ref/models/fields/#choices.
   - https://docs.djangoproject.com/en/1.11/ref/models/meta/.
   - https://docs.djangoproject.com/en/1.11/topics/db/managers/.
   - [Django Model Behaviors](http://blog.kevinastone.com/django-model-behaviors.html) Superbe!
   - [Supercharging Django Productivity](https://medium.com/eshares-blog/supercharging-django-productivity-at-eshares-8dbf9042825e)
