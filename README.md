# Dev Services

Docker-based local development services: **MySQL** and **PostgreSQL** databases, plus **Mailpit** for catching outgoing emails.

## Why a dockerized database instead of an in-memory one (like H2)?

- **The same database as in production:** same engine, same version, same SQL dialect. No query that works locally and breaks on deploy.
- **The same migrations everywhere:** the exact Flyway scripts run in dev, in tests and in production, so a broken migration fails on your machine first.
- **Bugs show up in dev, not in prod:** constraints, locking, transactions, error codes and engine-specific SQL behave exactly as they will in production.
- **Realistic performance:** query plans, indexes and slow queries can be checked locally (`EXPLAIN`) on the real engine.
- **Real tools:** inspect and debug the data with your usual database client. The data survives restarts.
- **One setup for dev and tests:** the same image runs locally (Dev Services) and in integration tests (Testcontainers).

> In-memory databases are fine for quick prototypes, or for tests that don't touch SQL. As soon as the code depends on real database behavior, use the real thing.

## Project structure

| Path | Description |
| --- | --- |
| `mysql/` | MySQL container setup with `docker-compose.yml`, `.env.example`, and `my.cnf` |
| `postgres/` | PostgreSQL container setup with `docker-compose.yml` and `.env.example` |
| `mailpit/` | Mailpit local email catcher (SMTP + web UI) with `docker-compose.yml` and `.env.example` |

## Quick start

1. Choose the service you want to run.
2. Go into that folder.
3. Copy the example environment file.
4. Start the container with Docker Compose.

```bash
cd mysql
cp .env.example .env
docker compose up -d
```

Or:

```bash
cd postgres
cp .env.example .env
docker compose up -d
```

Or:

```bash
cd mailpit
cp .env.example .env
docker compose up -d
```

Mailpit exposes SMTP on `localhost:1025` and the web UI on `http://localhost:8025` by default.

## Common commands

- Stop the container: `docker compose down`
- Reset a database with fresh data: `docker compose down -v && docker compose up -d`

## Service-specific documentation

- MySQL: [`mysql/README.md`](./mysql/README.md)
- PostgreSQL: [`postgres/README.md`](./postgres/README.md)
- Mailpit: [`mailpit/README.md`](./mailpit/README.md)
