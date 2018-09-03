- If there are 20+ models in a single app, think about ways to break it down into smaller apps, as it probably means your app is doing too much. In practice, we like to lower this number to no more than five models per app.

- At all costs, everyone should avoid multi-table inheritance (since each query on a child table requires joins with all parent tables) since it adds both confusion and substantial overhead. Instead of multi-table inheritance, use explicit OneToOneFields and ForeignKeys between models so you can control when joins are traversed.

- Additional links:
  - [https://docs.djangoproject.com/en/1.11/topics/db/models/#model-inheritance](https://docs.djangoproject.com/en/1.11/topics/db/models/#model-inheritance) (So goood).
