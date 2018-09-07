- When writing FBVs:
  - Less view code is better.
  - Never repeat code in view.
  - **Views should handle presentation logic. Try to keep business logic in models when possible, or in forms if you must.**
  - Keep your views simple.
  - Use them to write custom 403, 404, and 500 error handlers.
  - Complex nested-if blocks are to be avoided.
  
- There are times where we want to reuse code in views, but not tie it into global actions such as middleware or context processors. Starting in the introduction of this book, we advised creating utility functions that can be used across the project. Example 9.2, 9.3 (p. 119).
  - As with any powerful tool, decorators can be used the wrong way. Too many decorators can create their own form of obfuscation, making even complex class-based view hierarchies seem simple in comparison. **When using decorators, establish a limit of how many decorators can be set on a view and stick with it**.
  
- Since we’re repeatedly reusing functions inside functions, wouldn’t it be nice to easily recognize when this is being done? This is when we bring decorators into play.

- Every lesson we’ve learned about function-based views can be applied to what we begin to discuss next chapter, class-based views.

- Additional links:
  - Syntactic sugar.
  - [How to write obfuscated python](https://www.youtube.com/watch?v=eiaFUCp8dWc).
  - [Decorators Explained](https://www.jeffknupp.com/blog/2013/11/29/improve-your-python-decorators-explained/).
  - Obfuscation.
  - [Tail Call Optimization](https://stackoverflow.com/questions/310974/what-is-tail-call-optimization).
  - [Decorators and Functional Python] (http://www.brianholdefehr.com/decorators-and-functional-python).
  - [Decorator Cheat Sheet by author Daniel Roy Greenfeld](https://www.pydanny.com/python-decorator-cheatsheet.html).
