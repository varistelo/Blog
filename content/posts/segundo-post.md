---
title: "Docker Compose para Desenvolvimento Local"
date: 2025-07-20
tags: ["docker", "devops", "containers"]
categories: ["tutoriais"]
ShowToc: true
TocOpen: true
---

## Introdução

Vamos criar um ambiente de desenvolvimento local usando Docker Compose.

## O docker-compose.yml

```yaml
version: '3.8'

services:
  app:
    build: .
    ports:
      - "3000:3000"
    volumes:
      - .:/app
    environment:
      - NODE_ENV=development

  db:
    image: postgres:15
    environment:
      POSTGRES_DB: myapp
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: secret
    ports:
      - "5432:5432"
    volumes:
      - pgdata:/var/lib/postgresql/data

volumes:
  pgdata:
```

## Executando

```bash
docker-compose up -d
docker-compose logs -f app
```

## Comandos Úteis

```bash
# Parar tudo
docker-compose down

# Rebuild
docker-compose up --build

# Ver status
docker-compose ps
```

## Conclusão

Docker Compose simplifica muito o setup de desenvolvimento local.
