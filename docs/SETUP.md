# FlowPilot — Local Development Setup

This document provides a minimal local setup for the backend + PostgreSQL.

## Prerequisites

- Java 17+
- Docker + Docker Compose

## 1) Configure environment variables

```bash
cp .env.example .env
```

The backend reads database settings from:

- `DB_HOST` (default `localhost`)
- `DB_PORT` (default `5432`)
- `DB_NAME` (default `flowpilot`)
- `DB_USER` (default `flowpilot`)
- `DB_PASSWORD` (default `flowpilot`)

These are used when running with `SPRING_PROFILES_ACTIVE=local`.

## 2) Start PostgreSQL

```bash
docker compose up -d postgres
```

Optional DB UI (pgAdmin):

```bash
docker compose --profile tools up -d pgadmin
```

## 3) Run the backend

```bash
cd backend/flowpilot-backend
SPRING_PROFILES_ACTIVE=local ./gradlew bootRun
```

## 4) Verify

- Health: `http://localhost:8080/actuator/health`
- Ping: `http://localhost:8080/api/ping`
