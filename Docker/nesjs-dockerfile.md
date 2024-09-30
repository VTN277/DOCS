# Dockerfile Guide for NestJS Applications

## Basic Dockerfile

```dockerfile
# Use Node.js as the base image
FROM node:18

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

# Expose the port the app runs on
EXPOSE 3000

# Command to run the application
CMD ["npm", "run", "start:prod"]
```

## Explanation

1. `FROM node:18`: Uses Node.js 18 as the base image.
2. `WORKDIR /usr/src/app`: Sets the working directory inside the container.
3. `COPY package*.json ./`: Copies package.json and package-lock.json to optimize caching.
4. `RUN npm install`: Installs dependencies.
5. `COPY . .`: Copies the rest of the application source.
6. `RUN npm run build`: Builds the NestJS application.
7. `EXPOSE 3000`: Exposes port 3000 (default NestJS port).
8. `CMD ["npm", "run", "start:prod"]`: Starts the application in production mode.

## Use Cases and Examples

### 1. Development Environment

For a development environment, you might want to use volumes and enable hot-reloading:

```dockerfile
FROM node:18

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm", "run", "start:dev"]
```

Use with docker-compose:

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
```

### 2. Multi-stage Build for Smaller Production Image

```dockerfile
# Build stage
FROM node:18 AS build

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm ci

COPY . .

RUN npm run build

# Production stage
FROM node:18-alpine

WORKDIR /usr/src/app

COPY --from=build /usr/src/app/dist ./dist
COPY --from=build /usr/src/app/node_modules ./node_modules

EXPOSE 3000

CMD ["node", "dist/main"]
```

This approach creates a smaller production image by only including necessary files.

### 3. Including Environment Variables

For applications that require environment variables:

```dockerfile
FROM node:18

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm install

COPY . .

RUN npm run build

EXPOSE 3000

# Use an env file
COPY .env.production .env

CMD ["npm", "run", "start:prod"]
```

### 4. Custom Node.js Version and NPM Version

If you need specific versions:

```dockerfile
FROM node:18.15.0

WORKDIR /usr/src/app

# Install specific NPM version
RUN npm install -g npm@8.19.3

COPY package*.json ./

RUN npm ci

COPY . .

RUN npm run build

EXPOSE 3000

CMD ["npm", "run", "start:prod"]
```

### 5. Including Additional Services

For applications that require additional services like database migrations:

```dockerfile
FROM node:18

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm install

COPY . .

RUN npm run build

EXPOSE 3000

# Add a script to run migrations and start the app
COPY docker-entrypoint.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/docker-entrypoint.sh

ENTRYPOINT ["docker-entrypoint.sh"]
```

With a `docker-entrypoint.sh`:

```bash
#!/bin/sh
set -e

npm run migration:run
npm run start:prod
```

This setup allows you to run database migrations before starting the application.

## Best Practices

1. Use `.dockerignore` to exclude unnecessary files.
2. Leverage build cache by ordering Dockerfile instructions from least to most frequently changing.
3. Use multi-stage builds for production to reduce image size.
4. Specify exact versions of Node.js and npm for consistency.
5. Don't run the application as root; use `USER node` if possible.
6. Set NODE_ENV to "production" to optimize performance.

By following these practices and examples, you can create efficient Dockerfiles for various NestJS application scenarios.
