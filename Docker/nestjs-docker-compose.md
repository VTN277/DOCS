# Docker Compose with NestJS: A Comprehensive Guide

## Introduction
Docker Compose is a tool for defining and running multi-container Docker applications. When used with NestJS, it simplifies the process of setting up development and production environments.

## Basic Setup

### 1. Dockerfile
First, create a `Dockerfile` in your NestJS project root:

```dockerfile
FROM node:20-alpine

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm install

COPY . .

RUN npm run build

CMD ["npm", "run", "start:prod"]
```

### 2. docker-compose.yml
Create a `docker-compose.yml` file:

```yaml
version: '3.8'
services:
  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
```

### 3. Running the App
Run `docker-compose up --build` to start your NestJS application.

## Case Studies

### Case 1: NestJS with PostgreSQL

Update your `docker-compose.yml`:

```yaml
version: '3.8'
services:
  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - DB_HOST=db
      - DB_PORT=5432
      - DB_USERNAME=postgres
      - DB_PASSWORD=password
      - DB_NAME=mydb
    depends_on:
      - db

  db:
    image: postgres:14
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=password
      - POSTGRES_DB=mydb
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

### Case 2: NestJS with Redis for Caching

Add Redis to your `docker-compose.yml`:

```yaml
version: '3.8'
services:
  app:
    # ... (previous app configuration)
    environment:
      # ... (previous environment variables)
      - REDIS_HOST=redis
      - REDIS_PORT=6379
    depends_on:
      - db
      - redis

  db:
    # ... (previous db configuration)

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
```

### Case 3: NestJS with MongoDB and RabbitMQ

For a microservices architecture:

```yaml
version: '3.8'
services:
  app:
    # ... (previous app configuration)
    environment:
      # ... (previous environment variables)
      - MONGODB_URI=mongodb://mongo:27017/myapp
      - RABBITMQ_URI=amqp://rabbitmq:5672
    depends_on:
      - mongo
      - rabbitmq

  mongo:
    image: mongo:6
    volumes:
      - mongo_data:/data/db

  rabbitmq:
    image: rabbitmq:3-management
    ports:
      - "5672:5672"
      - "15672:15672"

volumes:
  mongo_data:
```

## Best Practices

1. Use environment variables for configuration.
2. Utilize `depends_on` to manage service start order.
3. Use volumes for persistent data storage.
4. Create separate Docker Compose files for development and production.
5. Use health checks to ensure services are ready before starting dependent services.

## Advanced Topics

### 1. Multi-stage Builds
Optimize your Dockerfile for smaller production images:

```dockerfile
FROM node:20-alpine AS builder

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm ci

COPY . .

RUN npm run build

FROM node:20-alpine

WORKDIR /usr/src/app

COPY --from=builder /usr/src/app/dist ./dist
COPY --from=builder /usr/src/app/node_modules ./node_modules

CMD ["node", "dist/main"]
```

### 2. Development Environment
Create a `docker-compose.dev.yml` for development:

```yaml
version: '3.8'
services:
  app:
    build:
      context: .
      dockerfile: Dockerfile.dev
    volumes:
      - .:/usr/src/app
      - /usr/src/app/node_modules
    command: npm run start:dev
    # ... (other configurations)
```

### 3. CI/CD Integration
Example `.gitlab-ci.yml` snippet for GitLab CI:

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
    - docker-compose run app npm run test

deploy:
  stage: deploy
  script:
    - docker-compose up -d
```

## Conclusion
Docker Compose simplifies the process of managing multi-container NestJS applications. By following these examples and best practices, you can create efficient, scalable, and easily deployable NestJS applications.
