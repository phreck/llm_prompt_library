# Testing Instructions for Django & Python

## Testing Philosophy

* **Test Pyramid**: More unit tests, fewer integration tests, minimal E2E tests
* **Test Behavior**: Test what the code does, not how it does it
* **Fast Feedback**: Tests should run quickly and provide clear failure messages
* **Isolation**: Each test should be independent and repeatable

## Django Testing Patterns

**Test Organization**:
```
tests/
├── test_models.py
├── test_views.py
├── test_services.py
├── test_api.py
├── factories.py
└── conftest.py
```

**Test Base Classes**:
* `TestCase` - For database-backed tests
* `TransactionTestCase` - For testing transactions
* `APITestCase` - For DRF API testing
* `SimpleTestCase` - For non-database tests

**Factory Pattern**:
```python
# Use factory_boy for test data
class UserFactory(factory.django.DjangoModelFactory):
    class Meta:
        model = User
    
    username = factory.Sequence(lambda n: f"user{n}")
    email = factory.LazyAttribute(lambda obj: f"{obj.username}@example.com")
```

## Test Types & Strategies

**Unit Tests**:
* Test individual functions/methods in isolation
* Mock external dependencies
* Focus on edge cases and error conditions

**Integration Tests**:
* Test component interactions
* Use real database but mock external services
* Test API endpoints end-to-end

**Performance Tests**:
* Use `django.test.utils.override_settings` for configuration
* Test database query counts with `assertNumQueries()`
* Profile slow tests and optimize

## Mocking & Fixtures

**Mocking External Services**:
```python
from unittest.mock import patch, Mock

@patch('myapp.services.external_api.call')
def test_service_with_external_call(self, mock_call):
    mock_call.return_value = {'status': 'success'}
    # Test implementation
```

**Database Fixtures**:
* Use factories instead of fixtures when possible
* Keep fixtures small and focused
* Use `pytest` fixtures for reusable test data

## Test Utilities

**Custom Assertions**:
* Create domain-specific assertion methods
* Use descriptive error messages
* Group related assertions

**Test Helpers**:
* Create utility functions for common test setup
* Use base test classes for shared functionality
* Implement custom test decorators when needed

## Coverage & Quality

**Coverage Requirements**:
* Aim for 80%+ code coverage
* Focus on critical business logic
* Don't test Django framework code

**Test Quality Metrics**:
* Fast execution time (< 1 second per test ideally)
* Clear, descriptive test names
* Minimal test setup and teardown
* Independent tests that can run in any order