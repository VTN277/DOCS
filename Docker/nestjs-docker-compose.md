# Docker Compose Cheatsheet for NestJS

This cheatsheet provides common Docker Compose commands and examples specifically tailored for NestJS applications.

## Basic Docker Compose Commands

- `docker-compose up`: Start all services defined in the docker-compose.yml file
- `docker-compose up -d`: Start all services in detached mode (run in background)
- `docker-compose down`: Stop and remove all containers, networks, and volumes
- `docker-compose ps`: List all running containers
- `docker-compose logs`: View output from containers
- `docker-compose build`: Build or rebuild services
- `docker-compose pull`: Pull images for services defined in docker-compose.yml

## NestJS-specific Docker Compose File

Here's a basic `docker-compose.yml` file for a NestJS application:

```yaml
version: '3.8'
services:
  nestjs-app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
    depends_on:
      - database

  database:
    image: postgres:13
    environment:
      POSTGRES_DB: nestjs
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

## Use Cases and Examples

1. **Development Environment**

   For a development setup, you might want to use volumes to sync your local code with the container:

   ```yaml
   version: '3.8'
   services:
     nestjs-app:
       build: .
       volumes:
         - .:/usr/src/app
         - /usr/src/app/node_modules
       ports:
         - "3000:3000"
       command: npm run start:dev
       environment:
         - NODE_ENV=development
   ```

   Run with: `docker-compose up`

2. **Testing**

   To run tests in a Docker environment:

   ```yaml
   version: '3.8'
   services:
     nestjs-test:
       build: .
       command: npm run test
       environment:
         - NODE_ENV=test
   ```

   Run with: `docker-compose -f docker-compose.test.yml up --abort-on-container-exit`

3. **Production with Multiple Services**

   For a production setup with a NestJS app, database, and Redis cache:

   ```yaml
   version: '3.8'
   services:
     nestjs-app:
       build: .
       ports:
         - "3000:3000"
       environment:
         - NODE_ENV=production
         - DB_HOST=database
         - REDIS_HOST=cache
       depends_on:
         - database
         - cache

     database:
       image: postgres:13
       environment:
         POSTGRES_DB: nestjs
         POSTGRES_USER: user
         POSTGRES_PASSWORD: password
       volumes:
         - postgres_data:/var/lib/postgresql/data

     cache:
       image: redis:6
       ports:
         - "6379:6379"

   volumes:
     postgres_data:
   ```

   Run with: `docker-compose up -d`

4. **Scaling Services**

   To scale your NestJS application:

   ```yaml
   version: '3.8'
   services:
     nestjs-app:
       build: .
       ports:
         - "3000-3010:3000"
       deploy:
         replicas: 5

     nginx:
       image: nginx:latest
       volumes:
         - ./nginx.conf:/etc/nginx/nginx.conf:ro
       ports:
         - "80:80"
       depends_on:
         - nestjs-app
   ```

   Run with: `docker-compose up -d --scale nestjs-app=5`

5. **Database Migrations**

   To run database migrations as part of your Docker Compose setup:

   ```yaml
   version: '3.8'
   services:
     nestjs-app:
       build: .
       ports:
         - "3000:3000"
       depends_on:
         - database

     database:
       image: postgres:13
       environment:
         POSTGRES_DB: nestjs
         POSTGRES_USER: user
         POSTGRES_PASSWORD: password

     migrations:
       build: .
       command: npm run migration:run
       depends_on:
         - database
   ```

   Run with: `docker-compose up migrations`

Remember to adjust these examples according to your specific NestJS application structure and requirements.
