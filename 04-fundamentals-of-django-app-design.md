-  The art of creating and maintaining a good Django app is that it should follow the truncated Unix philosophy according to Douglas McIlroy: ‘Write programs that do one thing and do it well.

- As a general rule, the app’s name should be a plural version of the app’s main model, but there are many good exceptions to this rule, blog being one of the most common ones.

- Don’t just consider the app’s main model, though. You should also consider how you want your URLs to appear when choosing a name. If you want your site’s blog to appear at http://www.example.com/weblog/, then consider naming your app weblog rather than blog, posts, or blogposts, even if the main model is Post.

- Try and keep your apps small. Remember, it’s better to have many small apps than to have a few giant apps.

- Common app modules (app-level):
  - admin.py
  - forms.py
  - management/
  - migrations/
  - models.py
  - templatetags/
  - tests/
  - urls.py
  - views.py
  
- Uncommon app modules (app-level):
  - api/ - the package we create for isolating the various modules needed when creating an api.
  - behaviors.py - An option for locating model mixins.
  - constants.py - A good name for placement of app-level settings.If there are enough of them involved in an app, breaking them out into their own module can add clarity to a project.
  - context_processors.py
  - decorators.py - Where we like to locate our decorators.
  - db/ - A package used in many projects for any custom model  elds or components.
  - exceptions.py
  - fields.py - commonly used for form fields, but is sometimes used for model  elds when there isn’t enough field code to justify creating a db/ package.
  - factories.py - Where we like to place our test data factories.
  - helpers.py - What we call helper functions. These are where we put code extracted from view and models to make them lighter. Synonymous with utils.py.
  - managers.py - When models.py grows too large, a common remedy is to move any custom model managers to this module.
  - middleware.py
  - signals.py - While we argue against providing custom signals, While we argue against providing custom signals, this can be a useful place to put them.
  - utils.py - Synonymous with helpers.py.
  - viewmixins.py - View modules and packages can be thinned by moving any view mixins to this module.
