# CloudNest

Subscription management for cloud services (INR, Indian locale). Stack: **React + Vite + Tailwind** (client), **Node + Express + Sequelize + MySQL** (server).

## Prerequisites

- Node.js 18+
- MySQL 8+ (server running locally or remote)

## 1. Create the database

In MySQL:

```sql
CREATE DATABASE cloudnest CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

## 2. Configure the server

```bash
cd server
copy .env.example .env
```

Edit `server/.env` and set `DB_USER`, `DB_PASSWORD`, and a strong `JWT_SECRET`.

## 3. Install dependencies

From the `cloudnest` folder:

```bash
cd server && npm install
cd ../client && npm install
```

## 4. Create tables and seed data

Still in `server` (with `.env` in place):

```bash
npm run db:sync
npm run db:seed
```

- `db:sync` runs `sequelize.sync({ alter: true })` and creates/updates all tables (use instead of `sequelize db:migrate` if you are not using the Sequelize CLI migrations folder).
- `db:seed` clears core tables and inserts sample data: admin, internal and portal users, products, variants, plans, discounts, taxes, templates, subscriptions, invoices, and payments.

### Seed logins

| Role    | Email                 | Password     |
|---------|----------------------|--------------|
| Admin   | admin@cloudnest.in   | Admin@123    |
| Internal| ops1@cloudnest.in    | Internal@123 |
| Portal  | customer1@example.com | Portal@123  |

## 5. Run the app

**Terminal 1 — API**

```bash
cd server
npm run dev
```

API: `http://localhost:5000` (health: `GET /api/health`).

**Terminal 2 — UI**

```bash
cd client
npm run dev
```

Open `http://localhost:5173`. The Vite dev server proxies `/api` to port 5000.

## Production build (client)

```bash
cd client
npm run build
```

Serve the `client/dist` folder with any static host; point API calls to your deployed server (update `client` axios base URL or proxy as needed).

## Notes

- Amounts are stored in **paise** (1 INR = 100 paise). The UI formats with `formatINR` (`en-IN`).
- Password reset uses OTP: request OTP, verify OTP, then set new password.
- For email delivery, configure SMTP variables in `server/.env`.
- Gmail app-password is supported directly by setting `SMTP_USER`, `SMTP_PASS`, and optionally `SMTP_FROM`.
- If mail variables are not configured, the server logs a warning and skips sending email.
