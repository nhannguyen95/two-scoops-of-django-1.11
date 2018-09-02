- Code is wrapped at 119 characters. Documentation, comments, docstrings are wrapped at 79 characters.

- Use four space hanging indentation rather than vertical alignment:
```
raise Error(
    'Here is a multine error message '
    'shortened for clarity.'
)
```

instead of:

```
raise Error('Here is a multine error message '
            'shortened for clarity.')
```

- Use single quotes for strings, or a double quote if the the string contains a single quote.

- Avoid use of “we” in comments, e.g. “Loop over” rather than “We loop over”.

- Use [isort](https://github.com/timothycrosley/isort#readme) to automate import sorting.

- Place all **import module** statements before **from module import objects** in each section.

- Break long lines using parentheses and indent continuation lines by 4 spaces. Include a trailing comma after the last import and put the closing parenthesis on its own line.

- Use a single blank line between the last import and any module level code, and use two blank lines above the first function or class.

- In Django views, the first parameter in a view function should be called **request**.

- The class Meta should appear after the fields are defined, with a single blank line separating the fields and the class definition.

- The order of model inner classes and standard methods should be as follows:
    - All database fields
    - Custom manager attributes
    - class Meta
    - def __str__()
    - def save()
    - def get_absolute_url()
    - Any custom methods
    
 - Links you need to check out:
 
    - docs.djangoproject.com/en/1.11/internals/.
