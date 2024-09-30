# In-Depth Guide to Docker Compose for NestJS Applications

## 1. Introduction to Docker Compose

Docker Compose is a tool for defining and running multi-container Docker applications. It allows you to use a YAML file to configure your application's services, networks, and volumes, making it easier to manage complex application stacks.

## 2. Basic Docker Compose Structure

A basic `docker-compose.yml` file for a NestJS application might look like this:

```yaml
version: '3'
services:
  nestjs-app:
    build: .
    ports:
      - "3000:3000"
    volumes:
      - .:/usr/src/app
      - /usr/src/app/node_modules
    environment:
      - NODE_ENV=development
```

This file:
1. Defines a service called `nestjs-app`
2. Builds the image using the Dockerfile in the current directory
3. Maps port 3000 from the container to the host
4. Mounts the current directory to `/usr/src/app` in the container
5. Sets an environment variable

## 3. Common Docker Compose Commands

- `docker-compose up`: Start the services
- `docker-compose down`: Stop and remove the containers
- `docker-compose build`: Build or rebuild the services
- `docker-compose logs`: View output from containers
- `docker-compose exec <service> <command>`: Run a command in a running container

## 4. Use Cases and Examples

### Use Case 1: NestJS with PostgreSQL and Redis

```yaml
version: '3'
services:
  nestjs-app:
    build: .
    ports:
      - "3000:3000"
    volumes:
      - .:/usr/src/app
      - /usr/src/app/node_modules
    environment:
      - DATABASE_HOST=postgres
      - DATABASE_PORT=5432
      - DATABASE_USER=postgres
      - DATABASE_PASSWORD=password
      - DATABASE_NAME=mydatabase
      - REDIS_HOST=redis
      - REDIS_PORT=6379
    depends_on:
      - postgres
      - redis

  postgres:
    image: postgres:13
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=password
      - POSTGRES_DB=mydatabase
    volumes:
      - postgres-data:/var/lib/postgresql/data

  redis:
    image: redis:6-alpine
    volumes:
      - redis-data:/data

volumes:
  postgres-data:
  redis-data:
```

This configuration:
1. Sets up a NestJS application with connections to PostgreSQL and Redis
2. Uses environment variables for database configuration
3. Sets up persistent volumes for both PostgreSQL and Redis

### Use Case 2: NestJS with MongoDB and RabbitMQ

```yaml
version: '3'
services:
  nestjs-app:
    build: .
    ports:
      - "3000:3000"
    volumes:
      - .:/usr/src/app
      - /usr/src/app/node_modules
    environment:
      - MONGODB_URI=mongodb://mongo:27017/mydatabase
      - RABBITMQ_URI=amqp://rabbitmq:5672
    depends_on:
      - mongo
      - rabbitmq

  mongo:
    image: mongo:4.4
    volumes:
      - mongo-data:/data/db

  rabbitmq:
    image: rabbitmq:3-management
    ports:
      - "15672:15672"
    volumes:
      - rabbitmq-data:/var/lib/rabbitmq

volumes:
  mongo-data:
  rabbitmq-data:
```

This setup:
1. Configures a NestJS app with MongoDB and RabbitMQ
2. Exposes RabbitMQ management interface on port 15672
3. Uses persistent volumes for both MongoDB and RabbitMQ

### Use Case 3: NestJS Microservices

```yaml
version: '3'
services:
  api-gateway:
    build: ./api-gateway
    ports:
      - "3000:3000"
    environment:
      - USER_SERVICE_HOST=user-service
      - USER_SERVICE_PORT=3001
      - PRODUCT_SERVICE_HOST=product-service
      - PRODUCT_SERVICE_PORT=3002

  user-service:
    build: ./user-service
    environment:
      - DATABASE_URI=mongodb://mongo:27017/users

  product-service:
    build: ./product-service
    environment:
      - DATABASE_URI=mongodb://mongo:27017/products

  mongo:
    image: mongo:4.4
    volumes:
      - mongo-data:/data/db

volumes:
  mongo-data:
```

This configuration:
1. Sets up a microservices architecture with an API gateway and two services
2. Uses environment variables for service discovery
3. Shares a MongoDB instance between services

### Use Case 4: NestJS with Elasticsearch and Kibana

```yaml
version: '3'
services:
  nestjs-app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - ELASTICSEARCH_NODE=http://elasticsearch:9200

  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.14.0
    environment:
      - discovery.type=single-node
    ports:
      - "9200:9200"
    volumes:
      - elasticsearch-data:/usr/share/elasticsearch/data

  kibana:
    image: docker.elastic.co/kibana/kibana:7.14.0
    ports:
      - "5601:5601"
    environment:
      - ELASTICSEARCH_HOSTS=http://elasticsearch:9200

volumes:
  elasticsearch-data:
```

This setup:
1. Configures a NestJS app with Elasticsearch for searching and indexing
2. Includes Kibana for visualization and management of Elasticsearch
3. Uses a persistent volume for Elasticsearch data

## 5. Advanced Docker Compose Features

### Environment Variables and .env Files

You can use a `.env` file to manage environment variables:

```yaml
version: '3'
services:
  nestjs-app:
    build: .
    ports:
      - "3000:3000"
    env_file: .env
```

`.env` file:
```
DATABASE_HOST=postgres
DATABASE_PORT=5432
DATABASE_USER=myuser
DATABASE_PASSWORD=mypassword
```

### Health Checks

You can add health checks to ensure your services are running correctly:

```yaml
version: '3'
services:
  nestjs-app:
    build: .
    ports:
      - "3000:3000"
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:3000/health"]
      interval: 30s
      timeout: 10s
      retries: 3
```

### Networks

You can define custom networks for your services:

```yaml
version: '3'
services:
  nestjs-app:
    build: .
    networks:
      - backend
      - frontend

  postgres:
    image: postgres:13
    networks:
      - backend

networks:
  backend:
  frontend:
```

## 6. Best Practices for Docker Compose with NestJS

1. Use environment variables for configuration to make your compose file more flexible.
2. Leverage health checks to ensure your services are running correctly.
3. Use volumes for persistent data storage.
4. Separate development and production configurations.
5. Use depends_on to manage service startup order.
6. Use networks to isolate services when necessary.

Example with best practices:

```yaml
version: '3'
services:
  nestjs-app:
    build:
      context: .
      dockerfile: ${DOCKERFILE:-Dockerfile.prod}
    ports:
      - "${PORT:-3000}:3000"
    volumes:
      - .:/usr/src/app
      - /usr/src/app/node_modules
    environment:
      - NODE_ENV=${NODE_ENV:-production}
      - DATABASE_HOST=${DATABASE_HOST:-postgres}
    depends_on:
      - postgres
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:3000/health"]
      interval: 30s
      timeout: 10s
      retries: 3
    networks:
      - backend

  postgres:
    image: postgres:13
    environment:
      - POSTGRES_USER=${DB_USER:-postgres}
      - POSTGRES_PASSWORD=${DB_PASSWORD:-password}
      - POSTGRES_DB=${DB_NAME:-mydatabase}
    volumes:
      - postgres-data:/var/lib/postgresql/data
    networks:
      - backend

volumes:
  postgres-data:

networks:
  backend:
```

This Docker Compose file incorporates several best practices, including the use of environment variables, health checks, volumes, and networks.

By understanding these examples and use cases, you can create efficient, scalable, and maintainable Docker Compose configurations for your NestJS applications across various scenarios and deployment needs.
