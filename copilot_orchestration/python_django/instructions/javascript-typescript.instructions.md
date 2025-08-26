# JavaScript/TypeScript Instructions for Django Integration

## Frontend-Backend Integration

**API Communication**:
* Use consistent URL patterns between Django and frontend routes
* Implement proper CORS configuration for Django
* Use Django's CSRF token for authenticated requests
* Handle Django's paginated responses consistently

**Authentication Integration**:
* Use Django's session authentication for traditional web apps
* Implement JWT or token authentication for SPAs
* Handle authentication state management in frontend
* Implement proper logout flows

## TypeScript with Django

**Type Definitions**:
```typescript
// Define types that match Django serializers
interface User {
  id: number;
  username: string;
  email: string;
  created_at: string;
}

interface ApiResponse<T> {
  count: number;
  next: string | null;
  previous: string | null;
  results: T[];
}
```

**API Client Pattern**:
```typescript
class ApiClient {
  private baseURL: string;
  
  async get<T>(endpoint: string): Promise<T> {
    const response = await fetch(`${this.baseURL}${endpoint}`, {
      headers: this.getHeaders(),
    });
    
    if (!response.ok) {
      throw new Error(`API Error: ${response.status}`);
    }
    
    return response.json();
  }
  
  private getHeaders(): Record<string, string> {
    return {
      'Content-Type': 'application/json',
      'X-CSRFToken': this.getCSRFToken(),
    };
  }
}
```

## Build Integration

**Static Files Management**:
* Configure Django to serve compiled assets
* Use Django's `collectstatic` with build outputs
* Implement proper cache busting strategies
* Handle development vs production asset serving

**Development Workflow**:
* Use Django's `runserver` alongside frontend dev server
* Configure proxy for API calls during development
* Implement hot reloading for both backend and frontend
* Use consistent linting and formatting across both codebases

## Frontend Architecture

**Component Organization**:
* Organize components by feature, not by type
* Implement proper state management
* Use consistent naming conventions
* Handle error states and loading states

**Performance Considerations**:
* Implement code splitting for large applications
* Use proper caching strategies
* Optimize bundle sizes
* Implement lazy loading where appropriate

## Testing Frontend Integration

**API Testing**:
* Mock Django API responses in frontend tests
* Test error handling and edge cases
* Implement integration tests for critical user flows
* Use tools like MSW for API mocking

**E2E Testing**:
* Test complete user workflows
* Use tools like Playwright or Cypress
* Test authentication flows
* Validate data persistence between frontend and backend