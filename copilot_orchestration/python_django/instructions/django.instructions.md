# Django Development Instructions for AI Agents

## Core Architecture Principles

**Separation of Concerns (SoC)**:
* **Models (`models.py`)**: Define data structure and serve as the single source of truth. Include model methods for business logic that operates on single instances.
* **Views (`views.py` or `api/views.py`)**: Handle request-response logic only. Keep views thin - delegate business logic to services, managers, or model methods.
* **Serializers (`serializers.py`)**: Define API representation using Django Rest Framework (DRF). Handle data validation, transformation, and nested relationships.
* **Services (`services.py`)**: Encapsulate complex business logic, external API calls, data manipulations, and operations spanning multiple models.
* **Forms (`forms.py`)**: Handle HTML form validation and cleaning. Use ModelForms when appropriate.
* **Managers (`managers.py`)**: Custom QuerySet methods and model-level database operations.
* **Utils (`utils.py`)**: Pure functions and utilities that don't fit elsewhere.

## Design Patterns & Best Practices

**Code Organization**:
* Favor composition over inheritance where appropriate
* Use well-known Python design patterns and OOP principles
* Implement the Repository pattern for complex data access
* Use Factory pattern for object creation when needed
* Apply Strategy pattern for interchangeable algorithms

**Django-Specific Patterns**:
* Use `select_related()` and `prefetch_related()` to optimize database queries
* Implement custom managers for reusable query logic
* Use `@property` decorators for computed model fields
* Leverage Django's signal system for decoupled event handling
* Use `get_object_or_404()` and `get_list_or_404()` for error handling

## Security Guidelines

* Always use Django's CSRF protection
* Validate and sanitize all user inputs
* Use Django's permission system and decorators (`@login_required`, `@permission_required`)
* Implement proper authentication (consider django-allauth for social auth)
* Use Django's built-in password validation
* Sanitize database queries to prevent SQL injection
* Use HTTPS in production settings

## API Development (DRF)

**ViewSets and Views**:
* Use `ModelViewSet` for full CRUD operations
* Use `ReadOnlyModelViewSet` for read-only endpoints
* Implement custom actions with `@action` decorator
* Use appropriate HTTP methods and status codes

**Serializers**:
* Use `ModelSerializer` when possible
* Implement custom validation with `validate_<field>()` methods
* Use nested serializers for related models
* Implement `to_representation()` for custom output formatting

**Permissions and Authentication**:
* Use DRF's permission classes (`IsAuthenticated`, `IsOwner`, etc.)
* Implement custom permissions when needed
* Use token-based authentication or JWT for APIs

## Database Best Practices

* Use migrations for all schema changes
* Add database indexes for frequently queried fields
* Use `db_index=True` in model fields when appropriate
* Implement soft deletes when data retention is required
* Use database constraints for data integrity
* Consider using `select_for_update()` for race condition prevention

## Testing Strategy

* Write unit tests for models, views, and services
* Use Django's `TestCase` and `TransactionTestCase`
* Implement factory classes with `factory_boy` for test data
* Use `pytest-django` for more flexible testing
* Test API endpoints with DRF's `APITestCase`
* Mock external services and APIs in tests

## Error Handling

* Use Django's built-in exception classes
* Implement custom exception classes when needed
* Use proper HTTP status codes in API responses
* Log errors appropriately with Django's logging framework
* Provide meaningful error messages for API consumers

## Performance Optimization

* Use database query optimization (`select_related`, `prefetch_related`, `only`, `defer`)
* Implement caching with Django's cache framework
* Use pagination for large datasets
* Consider using `bulk_create()` and `bulk_update()` for batch operations
* Profile database queries with Django Debug Toolbar

## File Structure Guidelines

```
myapp/
├── models/
│   ├── __init__.py
│   ├── user.py
│   └── product.py
├── api/
│   ├── __init__.py
│   ├── views.py
│   ├── serializers.py
│   └── urls.py
├── services/
│   ├── __init__.py
│   ├── user_service.py
│   └── email_service.py
├── tests/
│   ├── __init__.py
│   ├── test_models.py
│   ├── test_views.py
│   └── test_services.py
├── admin.py
├── apps.py
└── urls.py
```

## Code Quality Standards

* Follow PEP 8 style guidelines
* Use type hints for function parameters and return values
* Write docstrings for classes and complex methods
* Keep functions small and focused (single responsibility)
* Use meaningful variable and function names
* Implement proper logging instead of print statements

## Configuration Management

* Use environment variables for sensitive settings
* Separate settings files for different environments (dev, staging, prod)
* Use `django-environ` for environment variable management
* Never commit secrets to version control
* Use Django's `SECRET_KEY` properly

## Deployment Considerations

* Use `collectstatic` for static files in production
* Implement proper database connection pooling
* Use Celery for background tasks
* Set up proper monitoring and health checks
* Use gunicorn or uwsgi for production WSGI server

# Django-Specific Development Instructions

## Architecture & Organization

**Project Structure**:
```
project/
├── apps/
│   ├── core/           # Shared utilities, base classes
│   ├── users/          # User management
│   └── api/            # API endpoints
├── config/
│   ├── settings/       # Environment-specific settings
│   ├── urls.py
│   └── wsgi.py
└── requirements/       # Environment-specific dependencies
```

**App Structure** (for complex apps):
```
myapp/
├── models/
├── services/
├── api/
├── management/commands/
├── migrations/
├── tests/
├── admin.py
└── apps.py
```

## Model Design Patterns

**Base Models**:
```python
class TimeStampedModel(models.Model):
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    
    class Meta:
        abstract = True
```

**Custom Managers**:
* Use for common query patterns
* Implement business logic at the data layer
* Create chainable QuerySets

**Model Methods**:
* `__str__()` for human-readable representation
* `get_absolute_url()` for URL generation
* Business logic that operates on single instances

## View Patterns

**Class-Based Views**:
* Prefer CBVs for CRUD operations
* Use mixins for shared functionality
* Keep view logic minimal

**Function-Based Views**:
* Use for simple operations
* Better for complex business logic flows

## API Design (DRF)

**Serializer Patterns**:
* Use separate serializers for input/output when needed
* Implement field-level validation
* Use `SerializerMethodField` for computed fields

**ViewSet Organization**:
* Group related endpoints in ViewSets
* Use `@action` for custom endpoints
* Implement proper filtering and searching

## Database Optimization

**Query Optimization**:
* Always use `select_related()` for ForeignKey relationships
* Use `prefetch_related()` for ManyToMany and reverse FK
* Implement database indexes for filtered/ordered fields
* Use `only()` and `defer()` for field selection

**Migrations**:
* Keep migrations small and focused
* Use `RunPython` for data migrations
* Test migrations on production-like data

## Security Implementation

**Authentication & Authorization**:
* Use Django's built-in User model or extend it properly
* Implement proper permission checks in views
* Use DRF permissions for API endpoints

**Input Validation**:
* Validate all user inputs at form/serializer level
* Use Django's built-in validators
* Implement custom validators for business rules

## Configuration Management

**Settings Organization**:
```python
# settings/base.py - Common settings
# settings/development.py - Dev-specific
# settings/production.py - Prod-specific
# settings/testing.py - Test-specific
```

**Environment Variables**:
* Use `django-environ` for configuration
* Never hardcode sensitive data
* Use different configurations for different environments

## Django-Specific Best Practices

* Use Django's timezone-aware datetime handling
* Implement proper signal handling (avoid circular imports)
* Use Django's caching framework appropriately
* Leverage Django admin for internal tools
* Use Django's internationalization for multi-language support
