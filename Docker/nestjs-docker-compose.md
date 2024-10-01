# Concise Guide to Docker Compose in NestJS

## 1. Basic Docker Compose Commands

```bash
docker-compose up         # Start containers
docker-compose up -d      # Start in detached mode
docker-compose down       # Stop and remove containers
docker-compose logs       # View logs
docker-compose build      # Rebuild containers
docker-compose ps         # List containers
docker-compose exec <service> <command>  # Run command in container
```

## 2. Docker Compose File Structure

Basic `docker-compose.yml` for NestJS:

```yaml
version: '3.8'

services:
  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
    depends_on:
      - db

  db:
    image: postgres:13
    environment:
      POSTGRES_DB: nestjs
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
    volumes:
      - pgdata:/var/lib/postgresql/data

volumes:
  pgdata:
```

## 3. Use Cases and Examples

### Development Environment

`docker-compose.dev.yml`:

```yaml
version: '3.8'

services:
  app:
    build:
      context: .
      dockerfile: Dockerfile.dev
    volumes:
      - ./src:/app/src
    command: npm run start:dev
    ports:
      - "3000:3000"
      - "9229:9229"  # For debugging
    environment:
      - NODE_ENV=development

  db:
    image: postgres:13
    environment:
      POSTGRES_DB: nestjs_dev
      POSTGRES_USER: dev_user
      POSTGRES_PASSWORD: dev_password
    ports:
      - "5432:5432"

volumes:
  pgdata:
```

Run with: `docker-compose -f docker-compose.dev.yml up`

### Production Environment

`docker-compose.prod.yml`:

```yaml
version: '3.8'

services:
  app:
    build:
      context: .
      dockerfile: Dockerfile.prod
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
    depends_on:
      - db
    deploy:
      replicas: 3

  db:
    image: postgres:13
    environment:
      POSTGRES_DB: ${DB_NAME}
      POSTGRES_USER: ${DB_USER}
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    volumes:
      - pgdata:/var/lib/postgresql/data

  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro

volumes:
  pgdata:
```

Run with: `docker-compose -f docker-compose.prod.yml up -d`

### Microservices Architecture

`docker-compose.microservices.yml`:

```yaml
version: '3.8'

services:
  gateway:
    build: ./gateway
    ports:
      - "3000:3000"
    depends_on:
      - users
      - auth

  users:
    build: ./users-service
    environment:
      - DB_HOST=users-db

  auth:
    build: ./auth-service
    environment:
      - DB_HOST=auth-db

  users-db:
    image: postgres:13
    environment:
      POSTGRES_DB: users

  auth-db:
    image: postgres:13
    environment:
      POSTGRES_DB: auth

  redis:
    image: redis:6

networks:
  default:
    name: nestjs-network
```

Run with: `docker-compose -f docker-compose.microservices.yml up`

## 4. Best Practices

1. Use multi-stage builds in Dockerfiles
2. Store secrets in `.env` files (gitignored)
3. Use health checks for services
4. Separate dev and prod configurations
5. Use Docker networks for service isolation
6. Leverage Docker volumes for persistent data

## 5. Debugging

For debugging, add these to your `app` service:

```yaml
ports:
  - "9229:9229"
command: npm run start:debug
```

Then connect your IDE to localhost:9229.

## 6. Testing

For running tests in Docker:

```yaml
services:
  test:
    build: .
    command: npm run test
    environment:
      - NODE_ENV=test
```

Run with: `docker-compose run test`

## 7. Continuous Integration

Example `.gitlab-ci.yml` snippet:

```yaml
stages:
  - build
  - test
  - deploy

build:
  stage: build
  script:
    - docker-compose build

test:
  stage: test
  script:
    - docker-compose run test

deploy:
  stage: deploy
  script:
    - docker-compose -f docker-compose.prod.yml up -d
```

This concise guide covers the essentials of using Docker Compose with NestJS, including common commands, file structure, and practical examples for different scenarios. It also touches on best practices, debugging, testing, and CI integration.
