# FlexStock

[![CI](https://github.com/ZYRO-beta/flexstock/actions/workflows/ci.yml/badge.svg)](https://github.com/ZYRO-beta/flexstock/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![Node.js](https://img.shields.io/badge/node-%3E%3D18.0.0-brightgreen)](https://nodejs.org)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](./CONTRIBUTING.md)

> **FlexStock** — A self-hostable, white-label inventory management system. One codebase. Any business. Zero cloud dependency.

---

## Features

- **Product-agnostic** — configure product fields per category (pharmacy, clothing, hardware — all supported)
- **Real-time stock updates** via Socket.IO — all connected browsers sync instantly
- **Role-based access control** — Admin, Manager, Staff with enforced permissions
- **Dashboard** with charts, KPIs, and live low-stock alerts
- **Label & barcode printing** — PDF label sheets + optional thermal ZPL output
- **Inventory movements** — Stock In, Out, Adjustment, Transfer, with full audit trail
- **Optional Sales module** — POS checkout screen, receipt printing, sales history
- **PDF & Excel exports** — professional reports with business logo
- **Settings panel** — business name, logo, currency, locations, module toggles
- **Immutable audit logs** — sensitive changes are recorded with actor, time, request ID, and context
- **Security monitoring** — failed logins, permission denials, and blocked destructive actions are flagged
- **One-command install** — `./install.sh` handles everything

---

## Screenshots

> Dashboard with real-time KPIs, stock charts, and live low-stock alerts

---

## Quick Start

### Option A — Local Development (Dev Server)

```bash
git clone https://github.com/ZYRO-beta/flexstock.git
cd flexstock
cp .env.example .env

# 1. Spin up PostgreSQL in Docker
docker compose up -d postgres

# 2. Run migrations & seed data (first time only)
npm run prisma:migrate
npm run prisma:seed

# 3. Start client and server concurrently
npm run dev
```

- **Frontend**: http://localhost:5173
- **Backend API**: http://localhost:3000

To stop the running application:
- Terminate the dev server: `Ctrl + C`
- Stop the database container: `docker compose stop postgres`

### Option B — Automated Install (VPS / Linux Server)

```bash
git clone https://github.com/ZYRO-beta/flexstock.git
cd flexstock
cp .env.example .env
# Edit .env as needed
./install.sh
```

The install script handles dependency installation, database container creation, schema migrations, seeding, and frontend bundle building.

### Option C — Full Docker Containerized Stack

```bash
git clone https://github.com/ZYRO-beta/flexstock.git
cd flexstock
cp .env.example .env
docker compose -f docker-compose.prod.yml up -d
```

App runs at **http://localhost:3000**

---

### Default Credentials (After Seeding)

- **Admin**: `admin@flexstock.com` / `admin123`
- **Manager**: `manager@flexstock.com` / `manager123`
- **Staff**: `staff@flexstock.com` / `staff123`

> [!WARNING]
> Change all passwords immediately after first login in production environments.

---

## Docker & Database Management

```bash
# Start PostgreSQL database service
docker compose up -d postgres

# Check database container status & logs
docker compose ps
docker compose logs -f postgres

# Stop PostgreSQL database service
docker compose stop postgres

# Stop and remove containers and network
docker compose down
```

> [!TIP]
> **PostgreSQL Port Conflict (5432 vs 5433)**:
> If your host machine already runs another PostgreSQL service on port `5432`, FlexStock maps the database container to host port **`5433`** by default (`POSTGRES_PORT=5433` in `docker-compose.yml` and `.env`).
> If you prefer using `5432`, set `POSTGRES_PORT=5432` and update `DATABASE_URL` in `.env` accordingly.

---

## Configuration

All configuration is done via `.env`. Copy `.env.example` and edit:

```env
APP_NAME="FlexStock"
APP_URL=http://localhost:3000
PORT=3000
NODE_ENV=development

# Database (Default host port: 5433)
POSTGRES_PORT=5433
DATABASE_URL="postgresql://postgres:postgres_secure_password@localhost:5433/flexstock?schema=public"

# Auth Secrets
JWT_SECRET="super-secret-flexstock-jwt-key"
REFRESH_TOKEN_SECRET="super-secret-flexstock-refresh-key"

# Locale & Currency
CURRENCY_SYMBOL="PKR"
DATE_FORMAT="DD/MM/YYYY"
```

See [docs/configuration.md](./docs/configuration.md) for all available variables.

---

## Documentation

| Document | Description |
|---|---|
| [Architecture](./docs/architecture.md) | System design, data flow, real-time strategy |
| [API Reference](./docs/api.md) | All endpoints with request/response examples |
| [Deployment](./docs/deployment.md) | VPS, Docker, shared hosting setup |
| [Configuration](./docs/configuration.md) | All `.env` variables explained |
| [Custom Fields](./docs/custom-fields.md) | How the field builder works |
| [Label Printing](./docs/label-printing.md) | Tier 1 PDF + Tier 2 thermal setup |

---

## Contributing

Contributions are welcome! Please read [CONTRIBUTING.md](./CONTRIBUTING.md) for guidelines.

---

## Security

FlexStock now keeps an append-only audit trail for sensitive actions including users, categories, products, stock movements, sales, and settings. Failed authentication, permission denials, and blocked destructive actions are also recorded as security incidents so administrators can trace suspicious activity.

For security vulnerabilities, please see [SECURITY.md](./SECURITY.md) .

---

## License

[MIT](./LICENSE) © 2026 FlexStock Contributors
