# In-Depth Guide to Dockerfiles for NestJS Applications

## 1. Introduction to Dockerizing NestJS Applications

NestJS is a progressive Node.js framework for building efficient and scalable server-side applications. Dockerizing NestJS applications allows for consistent development, testing, and deployment across different environments.

## 2. Basic NestJS Dockerfile

Let's start with a basic Dockerfile for a NestJS application:

```dockerfile
# Use Node.js as the base image
FROM node:14

# Set the working directory
WORKDIR /usr/src/app

# Copy package.json and package-lock.json
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application code
COPY . .

# Build the application
RUN npm run build

# Expose the port the app runs on
EXPOSE 3000

# Command to run the application
CMD ["npm", "run", "start:prod"]
```

This Dockerfile:
1. Uses Node.js 14 as the base image
2. Sets the working directory
3. Copies package files and installs dependencies
4. Copies the rest of the application code
5. Builds the application
6. Exposes port 3000
7. Specifies the command to run the production build

## 3. Multi-Stage Build for Smaller Production Images

For a more optimized production image, we can use a multi-stage build:

```dockerfile
# Build stage
FROM node:14 AS builder

WORKDIR /usr/src/app

COPY package*.json ./
RUN npm install

COPY . .
RUN npm run build

# Production stage
FROM node:14-alpine

WORKDIR /usr/src/app

COPY --from=builder /usr/src/app/dist ./dist
COPY --from=builder /usr/src/app/node_modules ./node_modules

EXPOSE 3000

CMD ["node", "dist/main"]
```

This Dockerfile:
1. Uses a full Node.js image to build the application
2. Copies only the built application and production dependencies to a smaller Alpine-based image
3. Results in a significantly smaller production image

## 4. Development vs. Production Dockerfile

### Development Dockerfile

```dockerfile
FROM node:14

WORKDIR /usr/src/app

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm", "run", "start:dev"]
```

### Production Dockerfile

```dockerfile
FROM node:14-alpine AS builder

WORKDIR /usr/src/app

COPY package*.json ./
RUN npm install

COPY . .
RUN npm run build

FROM node:14-alpine

WORKDIR /usr/src/app

COPY --from=builder /usr/src/app/dist ./dist
COPY --from=builder /usr/src/app/node_modules ./node_modules

EXPOSE 3000

CMD ["node", "dist/main"]
```

Use `docker-compose.yml` to manage both environments:

```yaml
version: '3'
services:
  nestjs-app:
    build:
      context: .
      dockerfile: ${DOCKERFILE:-Dockerfile.prod}
    ports:
      - "3000:3000"
    volumes:
      - .:/usr/src/app
      - /usr/src/app/node_modules
    environment:
      - NODE_ENV=${NODE_ENV:-production}
```

Run development environment:
```
DOCKERFILE=Dockerfile.dev NODE_ENV=development docker-compose up
```

Run production environment:
```
docker-compose up
```

## 5. Use Cases and Examples

### Use Case 1: NestJS with TypeORM and PostgreSQL

```dockerfile
FROM node:14-alpine AS builder

WORKDIR /usr/src/app

COPY package*.json ./
RUN npm install

COPY . .
RUN npm run build

FROM node:14-alpine

WORKDIR /usr/src/app

COPY --from=builder /usr/src/app/dist ./dist
COPY --from=builder /usr/src/app/node_modules ./node_modules

# Install PostgreSQL client
RUN apk add --no-cache postgresql-client

EXPOSE 3000

CMD ["node", "dist/main"]
```

Docker Compose file (`docker-compose.yml`):

```yaml
version: '3'
services:
  nestjs-app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - DATABASE_HOST=postgres
      - DATABASE_PORT=5432
      - DATABASE_USER=postgres
      - DATABASE_PASSWORD=password
      - DATABASE_NAME=mydatabase
    depends_on:
      - postgres

  postgres:
    image: postgres:13
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=password
      - POSTGRES_DB=mydatabase
    volumes:
      - postgres-data:/var/lib/postgresql/data

volumes:
  postgres-data:
```

### Use Case 2: NestJS with MongoDB and Redis

```dockerfile
FROM node:14-alpine AS builder

WORKDIR /usr/src/app

COPY package*.json ./
RUN npm install

COPY . .
RUN npm run build

FROM node:14-alpine

WORKDIR /usr/src/app

COPY --from=builder /usr/src/app/dist ./dist
COPY --from=builder /usr/src/app/node_modules ./node_modules

EXPOSE 3000

CMD ["node", "dist/main"]
```

Docker Compose file (`docker-compose.yml`):

```yaml
version: '3'
services:
  nestjs-app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - MONGODB_URI=mongodb://mongo:27017/mydatabase
      - REDIS_HOST=redis
      - REDIS_PORT=6379
    depends_on:
      - mongo
      - redis

  mongo:
    image: mongo:4.4
    volumes:
      - mongo-data:/data/db

  redis:
    image: redis:6-alpine
    volumes:
      - redis-data:/data

volumes:
  mongo-data:
  redis-data:
```

### Use Case 3: NestJS with GraphQL

```dockerfile
FROM node:14-alpine AS builder

WORKDIR /usr/src/app

COPY package*.json ./
RUN npm install

COPY . .
RUN npm run build

FROM node:14-alpine

WORKDIR /usr/src/app

COPY --from=builder /usr/src/app/dist ./dist
COPY --from=builder /usr/src/app/node_modules ./node_modules

EXPOSE 3000

CMD ["node", "dist/main"]
```

For GraphQL, you don't need to change the Dockerfile, but you might want to add a volume for schema persistence in your `docker-compose.yml`:

```yaml
version: '3'
services:
  nestjs-graphql-app:
    build: .
    ports:
      - "3000:3000"
    volumes:
      - ./src/schema.gql:/usr/src/app/dist/schema.gql
```

## 6. Best Practices for NestJS Dockerfiles

1. Use multi-stage builds to keep production images small.
2. Leverage Docker layer caching by copying package files and installing dependencies before copying the rest of the code.
3. Use environment variables for configuration to make your Dockerfile more flexible.
4. Include only necessary files in your Docker build context using `.dockerignore`.
5. Run your NestJS application as a non-root user for better security.
6. Use health checks to ensure your application is running correctly.

Example with best practices:

```dockerfile
FROM node:14-alpine AS builder

WORKDIR /usr/src/app

COPY package*.json ./
RUN npm ci

COPY . .
RUN npm run build

FROM node:14-alpine

WORKDIR /usr/src/app

COPY --from=builder /usr/src/app/dist ./dist
COPY --from=builder /usr/src/app/node_modules ./node_modules

RUN addgroup -g 1001 -S nodejs
RUN adduser -S nestjs -u 1001

USER nestjs

EXPOSE 3000

HEALTHCHECK --interval=30s --timeout=30s --start-period=5s --retries=3 \
  CMD node healthcheck.js

CMD ["node", "dist/main"]
```

This Dockerfile incorporates several best practices, including using a non-root user and adding a health check.

By understanding these examples and use cases, you can create efficient, secure, and maintainable Dockerfiles for your NestJS applications across various scenarios and deployment needs.
