# NestJS Dockerfile Cheatsheet

This cheatsheet provides a comprehensive guide to creating Dockerfiles for NestJS applications, including common use cases and examples.

## Basic Dockerfile Structure

```dockerfile
# Base image
FROM node:14

# Create app directory
WORKDIR /usr/src/app

# Install app dependencies
COPY package*.json ./
RUN npm install

# Bundle app source
COPY . .

# Build the app
RUN npm run build

# Expose port
EXPOSE 3000

# Start the app
CMD [ "node", "dist/main" ]
```

## Use Cases and Examples

### 1. Development Environment

Use case: Setting up a development environment with hot-reloading.

```dockerfile
FROM node:14

WORKDIR /usr/src/app

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 3000

CMD [ "npm", "run", "start:dev" ]
```

### 2. Production Environment

Use case: Optimizing for a production environment.

```dockerfile
FROM node:14 AS builder

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

CMD [ "node", "dist/main" ]
```

### 3. Multi-stage Build with Testing

Use case: Including testing in the build process.

```dockerfile
FROM node:14 AS builder

WORKDIR /usr/src/app

COPY package*.json ./
RUN npm ci

COPY . .

RUN npm run test
RUN npm run build

FROM node:14-alpine

WORKDIR /usr/src/app

COPY --from=builder /usr/src/app/dist ./dist
COPY --from=builder /usr/src/app/node_modules ./node_modules

EXPOSE 3000

CMD [ "node", "dist/main" ]
```

### 4. Using Environment Variables

Use case: Configuring the app using environment variables.

```dockerfile
FROM node:14

WORKDIR /usr/src/app

COPY package*.json ./
RUN npm install

COPY . .

RUN npm run build

EXPOSE 3000

# Use environment variables
ENV NODE_ENV=production
ENV DATABASE_URL=mongodb://localhost:27017/myapp

CMD [ "node", "dist/main" ]
```

### 5. Using a Custom Start Script

Use case: Using a custom script to start the application.

```dockerfile
FROM node:14

WORKDIR /usr/src/app

COPY package*.json ./
RUN npm install

COPY . .

RUN npm run build

COPY start.sh ./
RUN chmod +x start.sh

EXPOSE 3000

CMD [ "./start.sh" ]
```

### 6. Using a Non-root User

Use case: Improving security by not running as root.

```dockerfile
FROM node:14

# Create app directory
WORKDIR /usr/src/app

# Install app dependencies
COPY package*.json ./
RUN npm ci

# Bundle app source
COPY . .

# Build the app
RUN npm run build

# Use a non-root user
RUN useradd -m myuser
USER myuser

EXPOSE 3000

CMD [ "node", "dist/main" ]
```

## Best Practices

1. Use specific versions for base images (e.g., `node:14` instead of `node:latest`).
2. Use multi-stage builds to keep final images small.
3. Use `.dockerignore` to exclude unnecessary files.
4. Run the application as a non-root user for improved security.
5. Use environment variables for configuration.
6. Optimize the layer caching by copying package.json first and installing dependencies before copying the rest of the app.

Remember to adapt these examples to your specific NestJS application needs and structure.
