---
layout: post
title: "Configure Postgres with Docker for local development"
date: 2021-09-07
author: "Konstantin Portnov"
header-img: "img/post-bg-2015.jpg"
tags:
    - Docker
    - Postgres
---

| Key |Value |
|-----|------|
| Postgres DBA username | `pgadmin` |
| Postgres DBA password | `pgpassword` |
| User username | `hello_user` |
| User password | `hello_password` |
| User DB | `hello_db` |

## Configure Postgres

### Create container

```shell
docker run -d --name hello_pgx \ 
    -e POSTGRES_USER=pgadmin \ 
    -e POSTGRES_PASSWORD=pgpassword \ 
    -p 5432:5432 postgres
```

### Connect to the container

```shell
docker exec -it hello_pgx psql -U pgadmin
```

### Apply migrations

```shell
docker exec -i hello_pgx psql -U pgadmin -t < migrations/000_setup-db.sql
docker exec -i hello_pgx psql -U hello_user -d hello_db -t < migrations/001_create-table.sql
```

## Docker

Remove unused containers

```shell
docker volume prune
```

## PGX

### Connection

- Variant 1: `postgres://hello_user:hello_password@localhost:5432/hello_db`
- Variant 2: `host=localhost port=5432 user=hello_user password=hello_password dbname=hello_db`