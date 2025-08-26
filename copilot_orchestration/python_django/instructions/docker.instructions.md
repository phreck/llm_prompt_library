# Docker Instructions for Django Applications

## Container Strategy

**Multi-Stage Builds**:
* Use separate stages for development and production
* Minimize final image size
* Cache dependencies effectively

**Base Image Selection**:
* Use official Python images (python:3.11-slim)
* Consider Alpine for smaller images
* Use specific tags, avoid 'latest'

## Dockerfile Patterns

**Development Dockerfile**:
```dockerfile
FROM python:3.11-slim

WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    && rm -rf /var/lib/apt/lists/*

# Copy requirements and install Python dependencies
COPY requirements/development.txt .
RUN pip install --no-cache-dir -r development.txt

# Copy application code
COPY . .

# Run development server
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
```

**Production Dockerfile**:
```dockerfile
FROM python:3.11-slim AS builder

WORKDIR /app
COPY requirements/production.txt .
RUN pip wheel --no-cache-dir --no-deps --wheel-dir /app/wheels -r production.txt

FROM python:3.11-slim

WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y \
    && rm -rf /var/lib/apt/lists/*

# Install Python dependencies
COPY --from=builder /app/wheels /wheels
COPY requirements/production.txt .
RUN pip install --no-cache-dir --find-links /wheels -r production.txt

# Create non-root user
RUN adduser --disabled-password --gecos '' appuser
USER appuser

# Copy application
COPY --chown=appuser:appuser . .

# Collect static files
RUN python manage.py collectstatic --noinput

CMD ["gunicorn", "--bind", "0.0.0.0:8000", "config.wsgi:application"]
```

## Docker Compose Patterns

**Development Environment**:
```yaml
version: '3.8'
services:
  web:
    build: .
    volumes:
      - .:/app
      - /app/node_modules
    ports:
      - "8000:8000"
    environment:
      - DEBUG=1
    depends_on:
      - db
      - redis

  db:
    image: postgres:14
    environment:
      POSTGRES_DB: myapp
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
    volumes:
      - postgres_data:/var/lib/postgresql/data/

  redis:
    image: redis:7-alpine
```

## Container Optimization

**Layer Caching**:
* Order Dockerfile commands by change frequency
* Copy requirements before source code
* Use .dockerignore to exclude unnecessary files

**Security**:
* Run containers as non-root user
* Use specific image tags
* Scan images for vulnerabilities
* Minimize installed packages

**Performance**:
* Use multi-stage builds for smaller images
* Implement health checks
* Configure appropriate resource limits
* Use init system for proper signal handling

## Deployment Patterns

**Environment Configuration**:
* Use environment variables for configuration
* Implement proper secrets management
* Use separate compose files for different environments

**Scaling Considerations**:
* Design stateless applications
* Use external databases and caches
* Implement proper logging for containerized apps
* Use reverse proxy for load balancing