---
title: nextjs-docker
tags:
---

# Next.js + Docker

## Dockerfile

```dockerfile
FROM node:14.5.4-alpine

WORKDIR /app

COPY package.json yarn.lock ./
RUN yarn install

COPY next.config.js ./next.config.js

COPY pages ./pages
COPY public ./public
COPY styles ./styles

CMD ["yarn", "dev"]
```

## docker-compose.yml

```yml
version: "0.1.0"

services:
  app:
    image: docker-nextjs-dev
    build: .
    ports:
      - "3000:3000"
```

接著執行：

```bash
docker-compose up --build --force-recreate
```