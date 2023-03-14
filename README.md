# Smart Adonis Api V1

API Template

## Features

* Adonis
* Postgres
* Unit Test
* Eslint
* Prettier
* Docker
* Typescript
* MVC
* Middleware
* Exception
* Migrations
* Factories
* Seeds
* Version
* Authentication
* CORS
* Lucid ORM

## Run Local in Docker

### Building image

```sh
docker build -t smart/adonis-api:v1 -f docker/Dockerfile.local .
```

### Create and Start Container

```sh
docker run -dit \
--name adonis-api \
-v `pwd`:/app \
-v adonis-api-nodemodules:/app/node_modules \
-p 3333:3000 smart/adonis-api:v1
```

### Postgres

```sh
docker run -dit \
--name adonis-api-postgres \
-e POSTGRES_PASSWORD=123456 \
-e POSTGRES_USER=dev-smart \
-e POSTGRES_DB=dev-adonis-api \
-p 5432:5432 postgres:14
```

### Network

```sh
docker network create smart-network
docker network connect smart-network adonis-api
docker network connect smart-network adonis-api-postgres
```

### Run migration

```sh
docker exec adonis-api node ace migration:run
docker exec adonis-api node ace generate:key
```
