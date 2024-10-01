# Dockerfile Guide for NestJS

## Basic Dockerfile

```dockerfile
# Use Node.js as the base image
FROM node:14

# Set working directory
WORKDIR /usr/src/app

# Copy package.json and package-lock.json
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy application source
COPY . .

# Build the application
RUN npm run build

# Expose port
EXPOSE 3000

# Start the application
CMD ["npm", "run", "start:prod"]
```

## Use Cases and Examples

1. Development Environment

```dockerfile
FROM node:14

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm", "run", "start:dev"]
```

Use case: Provides a consistent development environment across team members.

2. Production Deployment

```dockerfile
FROM node:14-alpine as builder

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm ci

COPY . .

RUN npm run build

FROM node:14-alpine

WORKDIR /usr/src/app

COPY --from=builder /usr/src/app/dist ./dist
COPY --from=builder /usr/src/app/node_modules ./node_modules

EXPOSE 3000

CMD ["node", "dist/main"]
```

Use case: Optimized for production deployment with a smaller image size.

3. Multi-stage Build with Test

```dockerfile
FROM node:14 as builder

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm ci

COPY . .

RUN npm run build

FROM node:14 as tester

WORKDIR /usr/src/app

COPY --from=builder /usr/src/app .

RUN npm run test

FROM node:14-alpine

WORKDIR /usr/src/app

COPY --from=builder /usr/src/app/dist ./dist
COPY --from=builder /usr/src/app/node_modules ./node_modules

EXPOSE 3000

CMD ["node", "dist/main"]
```

Use case: Includes a testing stage to ensure code quality before deployment.

4. Development with Hot Reload

```dockerfile
FROM node:14

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm", "run", "start:dev"]
```

Use case: Enables hot reloading for faster development iterations.

5. Production with Environment Variables

```dockerfile
FROM node:14-alpine

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm ci --only=production

COPY . .

RUN npm run build

EXPOSE 3000

CMD ["node", "dist/main"]
```

Use with docker-compose.yml:

```yaml
version: '3'
services:
  app:
    build: .
    environment:
      - DATABASE_URL=postgres://user:password@db:5432/dbname
    ports:
      - "3000:3000"
    depends_on:
      - db
  db:
    image: postgres
```

Use case: Configures the application with environment-specific settings.
6. Using Environment Variables for Configuration

```dockerfile
FROM node:14-alpine

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm ci --only=production

COPY . .

RUN npm run build

EXPOSE 3000

# Use environment variables for configuration
ENV NODE_ENV=production
ENV DATABASE_URL=postgres://user:password@db:5432/dbname
ENV REDIS_URL=redis://redis:6379

CMD ["node", "dist/main"]
```

Use case: Allows for flexible configuration across different environments without changing the Docker image.

7. Multi-stage Build with TypeORM Migrations

```dockerfile
FROM node:14-alpine as builder

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm ci

COPY . .

RUN npm run build

FROM node:14-alpine

WORKDIR /usr/src/app

COPY --from=builder /usr/src/app/dist ./dist
COPY --from=builder /usr/src/app/node_modules ./node_modules
COPY --from=builder /usr/src/app/ormconfig.json ./

# Add script to run migrations and start the app
COPY start-with-migrations.sh ./
RUN chmod +x start-with-migrations.sh

EXPOSE 3000

CMD ["./start-with-migrations.sh"]
```

```bash
#!/bin/sh
# start-with-migrations.sh
set -e

echo "Running database migrations..."
npx typeorm migration:run

echo "Starting the application..."
node dist/main
```

Use case: Ensures database schema is up-to-date before starting the application.

8 Using Docker Secrets

```dockerfile
FROM node:14-alpine

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm ci --only=production

COPY . .

RUN npm run build

EXPOSE 3000

# Use Docker secrets for sensitive information
CMD ["node", "dist/main.js", "--db-password-file", "/run/secrets/db-password"]
```

Use with docker-compose.yml:

```yaml
version: '3.7'
services:
  app:
    build: .
    secrets:
      - db-password
    environment:
      - NODE_ENV=production
      - DATABASE_URL=postgres://user:@db:5432/dbname
    ports:
      - "3000:3000"
  db:
    image: postgres
    secrets:
      - db-password
    environment:
      POSTGRES_PASSWORD_FILE: /run/secrets/db-password

secrets:
  db-password:
    file: ./db-password.txt
```

Use case: Securely manages sensitive information like database passwords.

9. Multi-stage Build with Tests and Code Coverage

```dockerfile
FROM node:14 as builder

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm ci

COPY . .

RUN npm run build

FROM node:14 as tester

WORKDIR /usr/src/app

COPY --from=builder /usr/src/app .

RUN npm run test
RUN npm run test:cov

FROM node:14-alpine

WORKDIR /usr/src/app

COPY --from=builder /usr/src/app/dist ./dist
COPY --from=builder /usr/src/app/node_modules ./node_modules
COPY --from=tester /usr/src/app/coverage ./coverage

EXPOSE 3000

CMD ["node", "dist/main"]
```

Use case: Runs tests and generates code coverage reports as part of the build process.

10. Development Environment with Debugging

```dockerfile
FROM node:14

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000 9229

CMD ["npm", "run", "start:debug"]
```

Use with docker-compose.yml:

```yaml
version: '3'
services:
  app:
    build: .
    volumes:
      - .:/usr/src/app
      - /usr/src/app/node_modules
    ports:
      - "3000:3000"
      - "9229:9229"
    environment:
      - NODE_ENV=development
    command: npm run start:debug
```

Use case: Enables debugging capabilities in a dockerized development environment.

## Best Practices for Environment-Specific Configurations

1. Use environment variables for configuration that changes between environments.
2. Store sensitive information in Docker secrets or secure environment management tools.
3. Use different Docker Compose files for development, testing, and production environments.
4. Implement health checks to ensure services are ready before the application starts.
5. Use multi-stage builds to separate concerns (building, testing, production runtime).
6. Implement logging strategies that work well with containerized environments (e.g., logging to stdout/stderr).
7. Consider using init systems like dumb-init to handle signal forwarding and zombie process reaping.
8. Use `.dockerignore` to exclude environment-specific files that shouldn't be in the image.
9. Implement proper error handling and graceful shutdowns to work well with container orchestration systems.
10. Use CI/CD pipelines to automate building and testing Docker images for different environments.

By implementing these practices and using the provided examples, you can create flexible, secure, and efficient Dockerfiles for your NestJS applications that work seamlessly across different environments.
