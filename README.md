# READ ME

## Description

[Nest] A API Project for a Authentication and Authorization using JWT and Passport. Use TypeORM to connect with a MySQL database. Cookies are used to store the JWT token.

## Create a Project

```bash
$ nest new nestjs-authentication
```

## Installation

```bash
$ npm install
```

## Running the app

```bash
# development
$ npm run start

# watch mode
$ npm run start:dev

# production mode
$ npm run start:prod
```

## Test

```bash
# unit tests
$ npm run test

# e2e tests
$ npm run test:e2e

# test coverage
$ npm run test:cov
```

## Create Dockerfile

```bash
$ touch Dockerfile
```

```Dockerfile
FROM node:16-alpine3.11

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm", "run", "start:dev"]
```

- build and run without docker-compose

```bash
$ docker build -t nestjs-authentication .
$ docker run -p 8000:3000 nestjs-authentication
```
