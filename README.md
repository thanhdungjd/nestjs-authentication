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

## Create docker-compose.yml

```bash
$ touch docker-compose.yml
```

```yml
version: '3'
services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - '8000:3000'
    volumes:
      - .:/app
    depends_on:
      - db

  db:
    image: mysql:8.0.19
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: nest_auth
      MYSQL_USER: nest_auth
      MYSQL_PASSWORD: nest_auth
    volumes:
      - ./data:/var/lib/mysql
    ports:
      - '33066:3306'
```

- build and run with docker-compose

```bash
$ docker-compose up -d --build
```

## Generate User Module

- generate user module

```bash
nest g module user
nest g controller user
nest g service user
```

- remove user.controller.spec.ts and user.service.spec.ts

```bash
rm src/user/user.controller.spec.ts
rm src/user/user.service.spec.ts
```

- create user entity

```bash
touch src/user/user.ts
```

- install typeorm

```bash
npm install --save @nestjs/typeorm typeorm mysql2
```

```ts
import { Entity, Column, PrimaryGeneratedColumn } from 'typeorm';

@Entity({ name: 'users' })
export class User {
  @PrimaryGeneratedColumn()
  id: number;

  @Column({ length: 500 })
  name: string;

  @Column({ length: 500 })
  email: string;

  @Column({ length: 500 })
  password: string;
}
```

- update app.module.ts

```ts
import { Module } from '@nestjs/common';
import { TypeOrmModule } from '@nestjs/typeorm';
import { User } from './user/user';

@Module({
    imports: [
        TypeOrmModule.forRoot({
            type: 'mysql',
            host: 'db',
            port: 3306,
            username: 'nest_auth',
            password: 'nest_auth',
            database: 'nest_auth',
            entities: [User],
            synchronize: true,
        }),
    ],

})
```

- If you change the user entity, maybe you will get this error:

```bash
# nestjs-authentication-db-1   | mbind: Operation not permitted
```
