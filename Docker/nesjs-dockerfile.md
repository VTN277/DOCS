# Dockerfile Cheat Sheet for NestJS Applications

This cheat sheet provides examples and explanations for creating Dockerfiles for NestJS applications.

## Basic Dockerfile

```dockerfile
# Use an official Node runtime as the base image
FROM node:18

# Set the working directory in the container
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

## Explanation:
- `FROM node:18`: Uses Node.js 18 as the base image.
- `WORKDIR /usr/src/app`: Sets the working directory inside the container.
- `COPY package*.json ./`: Copies package.json and package-lock.json to optimize caching.
- `RUN npm install`: Installs dependencies.
- `COPY . .`: Copies the rest of the application code.
- `RUN npm run build`: Builds the NestJS application.
- `EXPOSE 3000`: Exposes port 3000 (default NestJS port).
- `CMD ["npm", "run", "start:prod"]`: Starts the application in production mode.

## Multi-Stage Build

```dockerfile
# Build stage
FROM node:18 AS build

WORKDIR /usr/src/app

COPY package*.json ./
RUN npm install

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

## Explanation:
- Uses a multi-stage build to create a smaller production image.
- The build stage compiles the TypeScript code.
- The production stage uses a lightweight Alpine-based Node.js image.
- Only the compiled code and production dependencies are copied to the final image.

## Development Dockerfile

```dockerfile
FROM node:18

WORKDIR /usr/src/app

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm", "run", "start:dev"]
```

## Explanation:
- Similar to the basic Dockerfile, but doesn't build the app.
- Uses `npm run start:dev` to run the app in development mode with hot-reloading.

## Tips:
1. Use `.dockerignore` to exclude unnecessary files (e.g., node_modules, .git).
2. Consider using environment variables for configuration.
3. For production, use multi-stage builds to reduce image size.
4. Use Docker Compose for managing multi-container NestJS applications.

Remember to adjust the Dockerfile based on your specific NestJS application structure and requirements.
