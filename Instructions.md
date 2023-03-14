# Instructions

Creation Step by Step

## Update global npm

```sh
npm -g outdate
npm -g update
npm install -g npm@9.6.1
```

## Create Project

```sh
npm init adonis-ts-app@latest api-adonis-docker
sudo chmod -R 777 api-adonis-docker/
cd api-adonis-docker
npm run dev
```

## Create .dockerignore

```sh
touch .dockerignore

# Adonis default .gitignore ignores
node_modules
build
coverage
.vscode
.DS_STORE
# .env

# Additional .gitignore ignores (any custom file you wish)
.idea

# Additional good to have ignores for dockerignore
Dockerfile*
docker-compose*
.dockerignore
*.md
.git
.gitignore
```

## Create Docker Container

```sh
mkdir docker && touch docker/Dockerfile.local
```

## Dockerfile.local

```sh
ARG NODE_IMAGE=node:18.15-alpine3.17

FROM $NODE_IMAGE AS base
RUN apk --no-cache add dumb-init
RUN mkdir -p /app && chown node:node /app
WORKDIR /app
USER node

FROM base AS development
ENV NODE_ENV=development
ENV PORT=3000
ENV HOST=0.0.0.0
ENV APP_KEY=fOqCggLg7Ch5Pe71jT5-TKz1SFdn0QjK
ENV DRIVE_DISK=local

COPY --chown=node:node ./package*.json ./
RUN npm install

EXPOSE $PORT

CMD [ "npm", "run", "dev" ]
```

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
docker network inspect smart-network
```

### Dependencies

```sh
npm i @adonisjs/lucid pg
```

### Config Lucid

```sh
node ace configure @adonisjs/lucid
```

### Config .Env

```sh
PG_HOST=adonis-api-postgres
```

### Update Dependencies into Container

```sh
docker exec adonis-api npm install
docker restart adonis-api
```

### Check DB connection

Change `healthCheck` to `true` in app/database.ts

```js
import HealthCheck from '@ioc:Adonis/Core/HealthCheck'
const report = await HealthCheck.getReport()
return report.healthy ? response.ok(report) : response.badRequest(report)
```

## Config VSCode

```sh
mkdir .vscode && touch .vscode/settings.json
{
  "editor.tabSize": 2,
  "editor.insertSpaces": true,
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
}
```

## Git

```sh
git init
git remote add origin git@github.com:edinaldoFelipe/adonis-api.git
git add .
git commit -m "Initial Commit"
git push -u origin main
```

## Adonis Commands

### Create Model with migration + factory

```sh
node ace make:model User -cf
```

## Controllers

### Create

```sh
node ace make:controller User
```

## Postgres Commands

```sh
su - postgres
GRANT ALL ON dev_daraknows to daraknows-dev
psql -U daraknows-dev -d dev_daraknows
CREATE TABLE "users"( id varchar(40) PRIMARY KEY NOT NULL );
```
