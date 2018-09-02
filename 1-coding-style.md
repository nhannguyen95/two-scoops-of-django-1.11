Code is wrapped at 119 characters. Documentation, comments, docstrings are wrapped at 79 characters.

Use four space hanging indentation rather than vertical alignment:
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
