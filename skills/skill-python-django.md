---
name: skill-python-django
description: Python and Django development patterns following Python best practices and Django conventions
when_to_use: Detected when working with .py files, manage.py, Django views, models, or Django projects
version: 1.2
---

# Python & Django Development Skill

## Core Principle
**Follow Django conventions and Python best practices first.** If Django has a documented way to do something, use it.

## Python Standards
- Follow PEP 8 for code style
- Use type hints for function parameters and return values
- Use descriptive variable and function names
- Prefer list comprehensions over map/filter when readable
- Use f-strings for string formatting
- Follow the Zen of Python (PEP 20)

## Django Conventions

### Models
- Use singular model names (`User`, `Post`, `Order`)
- Follow Django's field naming conventions
- Use `related_name` for reverse relationships
- Implement `__str__` method for all models
- Use Django's built-in fields when possible

### Views
- Use class-based views for complex logic
- Use function-based views for simple operations
- Follow RESTful naming conventions
- Implement proper permission checks
- Use Django's generic views when appropriate

### URLs
- Use descriptive URL patterns
- Use kebab-case for URL paths
- Use snake_case for URL names
- Group related URLs in app-specific `urls.py`

### Templates
- Use Django template language (DTL)
- Organize templates by app
- Use template inheritance
- Implement proper context processors
- Use `{% static %}` for static files

## File Structure
```
project/
├── app_name/
│   ├── models.py          # Database models
│   ├── views.py           # View logic
│   ├── urls.py            # URL patterns
│   ├── forms.py           # Form definitions
│   ├── admin.py           # Admin configuration
│   ├── tests.py           # Tests
│   └── templates/         # Templates
│       └── app_name/
├── static/                # Static files
└── manage.py             # Django management script
```

## Best Practices
1. **Security**: Use Django's built-in security features
2. **Performance**: Optimize database queries with select_related/prefetch_related
3. **Testing**: Write tests for models, views, and forms
4. **Documentation**: Use docstrings for modules, classes, and functions
5. **Code Quality**: Use linters (flake8, pylint) and formatters (black)

## Common Patterns

### Model Definition
```python
from django.db import models
from django.contrib.auth.models import User

class Post(models.Model):
    title = models.CharField(max_length=200)
    content = models.TextField()
    author = models.ForeignKey(
        User,
        on_delete=models.CASCADE,
        related_name='posts'
    )
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    class Meta:
        ordering = ['-created_at']
        verbose_name_plural = 'posts'

    def __str__(self) -> str:
        return self.title
```

### Class-Based View
```python
from django.views.generic import ListView, DetailView
from .models import Post

class PostListView(ListView):
    model = Post
    template_name = 'blog/post_list.html'
    context_object_name = 'posts'
    paginate_by = 10

    def get_queryset(self):
        return Post.objects.select_related('author').filter(
            published=True
        )
```

## Testing
- Use Django's TestCase for database-dependent tests
- Use SimpleTestCase for non-database tests
- Follow the arrange-act-assert pattern
- Use factories or fixtures for test data
- Test models, views, forms, and custom logic

## Dependencies
- Python 3.10+ (latest stable version)
- Django 4.0+ (latest LTS or stable version)

## References
- [Django Documentation](https://docs.djangoproject.com/)
- [Python Documentation](https://docs.python.org/)
- [PEP 8 Style Guide](https://pep8.org/)

---
*Use this skill for all Python/Django development*
