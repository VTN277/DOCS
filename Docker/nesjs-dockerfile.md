# Dockerfile Guide for NestJS Applications

## Basic Dockerfile

Here's a basic Dockerfile for a NestJS application:

```dockerfile
# Build stage
FROM node:18-alpine AS build

WORKDIR /app

COPY package*.json ./
RUN npm ci

COPY . .
RUN npm run build

# Production stage
FROM node:18-alpine

WORKDIR /app

COPY --from=build /app/dist ./dist
COPY --from=build /app/node_modules ./node_modules
COPY package*.json ./

EXPOSE 3000
CMD ["node", "dist/main"]
```

## Use Cases and Examples

### 1. Development Environment

For development, you might want to use a different Dockerfile:

```dockerfile
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 3000
CMD ["npm", "run", "start:dev"]
```

### 2. Using Docker Compose

Create a `docker-compose.yml` file for local development:

```yaml
version: '3.8'
services:
  app:
    build:
      context: .
      dockerfile: Dockerfile.dev
    volumes:
      - .:/app
      - /app/node_modules
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=development
```

### 3. Multi-stage Build with Testing

Include testing in your build process:

```dockerfile
# Build stage
FROM node:18-alpine AS build

WORKDIR /app

COPY package*.json ./
RUN npm ci

COPY . .
RUN npm run build

# Test stage
FROM build AS test
RUN npm run test

# Production stage
FROM node:18-alpine

WORKDIR /app

COPY --from=build /app/dist ./dist
COPY --from=build /app/node_modules ./node_modules
COPY package*.json ./

EXPOSE 3000
CMD ["node", "dist/main"]
```

### 4. Using Environment Variables

Utilize environment variables for configuration:

```dockerfile
# ... (previous stages)

FROM node:18-alpine

WORKDIR /app

COPY --from=build /app/dist ./dist
COPY --from=build /app/node_modules ./node_modules
COPY package*.json ./

EXPOSE 3000

ENV NODE_ENV=production
ENV DATABASE_URL=postgres://user:password@host:5432/db

CMD ["node", "dist/main"]
```

### 5. Health Check

Add a health check to your Dockerfile:

```dockerfile
# ... (previous stages)

FROM node:18-alpine

WORKDIR /app

COPY --from=build /app/dist ./dist
COPY --from=build /app/node_modules ./node_modules
COPY package*.json ./

EXPOSE 3000

HEALTHCHECK --interval=30s --timeout=30s --start-period=5s --retries=3 \
  CMD curl -f http://localhost:3000/health || exit 1

CMD ["node", "dist/main"]
```

Remember to implement a `/health` endpoint in your NestJS application.

## Best Practices

1. Use multi-stage builds to keep the final image small
2. Utilize `.dockerignore` to exclude unnecessary files
3. Run the application as a non-root user for security
4. Use specific versions for base images
5. Optimize caching by copying package.json files first
6. Set appropriate environment variables
7. Implement health checks for container orchestration

By following these practices and examples, you can create efficient and secure Dockerfiles for your NestJS applications.
