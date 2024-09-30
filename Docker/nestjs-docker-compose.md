# Docker Compose Cheat Sheet for NestJS Applications

This cheat sheet provides examples and explanations for using Docker Compose with NestJS applications.

## Basic docker-compose.yml

```yaml
version: '3.8'
services:
  nestjs-app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
```

### Explanation:
- `version: '3.8'`: Specifies the Docker Compose file version.
- `services`: Defines the services (containers) in your application.
- `nestjs-app`: The name of your NestJS service.
- `build: .`: Builds the image using the Dockerfile in the current directory.
- `ports`: Maps port 3000 on the host to port 3000 in the container.
- `environment`: Sets environment variables.

## Development Environment

```yaml
version: '3.8'
services:
  nestjs-app:
    build:
      context: .
      dockerfile: Dockerfile.dev
    volumes:
      - .:/usr/src/app
      - /usr/src/app/node_modules
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=development
    command: npm run start:dev
```

### Explanation:
- `build`: Specifies a custom Dockerfile for development.
- `volumes`: Mounts the current directory to `/usr/src/app` in the container for live code reloading.
- `command`: Overrides the CMD in the Dockerfile to run in development mode.

## With Database (PostgreSQL)

```yaml
version: '3.8'
services:
  nestjs-app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - DB_HOST=postgres
      - DB_PORT=5432
      - DB_USERNAME=user
      - DB_PASSWORD=password
      - DB_DATABASE=mydb
    depends_on:
      - postgres

  postgres:
    image: postgres:13
    environment:
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=password
      - POSTGRES_DB=mydb
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

### Explanation:
- Adds a PostgreSQL service to the composition.
- `depends_on`: Ensures the database starts before the NestJS app.
- `volumes`: Creates a named volume for persistent database storage.

## With Redis for Caching

```yaml
version: '3.8'
services:
  nestjs-app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - REDIS_HOST=redis
      - REDIS_PORT=6379
    depends_on:
      - redis

  redis:
    image: redis:6-alpine
```

### Explanation:
- Adds a Redis service for caching or session storage.
- Uses the lightweight Alpine-based Redis image.

## Development Environment with Hot Reload

```yaml
version: '3.8'
services:
  nestjs-app:
    build:
      context: .
      dockerfile: Dockerfile.dev
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

### Explanation:
- Maps port 9229 for Node.js debugging.
- Uses `npm run start:debug` for hot reloading and debugging support.

## Tips:
1. Use environment variables or `.env` files for sensitive information.
2. Create separate compose files for different environments (e.g., `docker-compose.prod.yml`).
3. Use `docker-compose up --build` to rebuild images when Dockerfile changes.
4. For production, consider using Docker Swarm or Kubernetes for orchestration.
5. Use `healthcheck` to ensure services are ready before depending on them.

Remember to adjust the Docker Compose configurations based on your specific NestJS application requirements and architecture.
