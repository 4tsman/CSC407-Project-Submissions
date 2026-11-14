# School Fees Payment Management System

A full-stack school fees management application built for **CSC 407: Building Web Applications — Web Application Security, Testing, Deployment and Review**. The project is presented as a fictional university, Kingsgate University, and demonstrates student fee payments alongside administrative payment and fee management.

**Live demo:** [School Fees Payment System](https://kingsgate-beta.vercel.app/)

## Student Information


| **Name** | Dunu Chisom Benjamin |
| **Matriculation number** | 23/sci01/059 |
| **Course** | CSC 407 |
| **Project** | School Fees Payment Management System |

## Project Overview

Students can view fees assigned to them, make full or installment payments using a simulated payment gateway, check payment history, and access receipts with QR-based verification. Administrative users can manage students, fee structures and payments, review debtors, generate reports, and inspect audit records.

The project also includes dedicated pages showcasing its security, testing, deployment, and review work. It is an academic demonstration: payments use a sandbox gateway and are **not real transactions**.

## Features

- **Student portal:** fee dashboard, fee breakdown, installment payments, payment history, receipts, profile, notifications, and session management.
- **Bursary and system administration:** student records, fee structures, payment review, debtor lists, reports, audit log, roles, and system health.
- **Authentication:** email verification, password reset, staff one-time codes, role-based access, and server-managed sessions.
- **Payment simulation:** sandbox card payments, payment status checks, idempotency protection, and QR-verifiable receipts.
- **Security controls:** password hashing, login throttling and lockout, CSRF protection, origin checks, request validation, security headers, and a tamper-evident audit log.
- **Responsive interface:** light/dark themes and layouts for student, staff, and public pages.

## Technology

- **Frontend:** React 19, Vite, Tailwind CSS 4, React Router, Recharts
- **Backend:** Node.js, Express 5
- **Database:** Prisma ORM with SQLite for local development
- **Testing:** Node.js test runner and Supertest
- **Deployment configuration:** Vercel (`vercel.json`)

## Run Locally

### Prerequisites

- Node.js (version 22 recommended)
- npm

### Install and start

From the project root:

```bash
npm install
npm run dev
```

Open the frontend at [http://localhost:5173](http://localhost:5173). The API runs at `http://localhost:4000`; Vite proxies `/api` and `/sandbox-gateway` requests to it. On first startup, the server creates `server/.env` from `server/.env.example`, initializes the SQLite database, and seeds demo data.

### Useful commands

| Command | Description |
|---|---|
| `npm run dev` | Start the API and frontend for development |
| `npm run build` | Build the frontend into `dist/` |
| `npm run preview` | Preview the production frontend build |
| `npm test` | Run the API test suite against a fresh test database |
| `npm run db:reset` | Reset and reseed the local demo database |
| `npm start` | Start the API server |

## Demo Accounts

These accounts are created by the local database seed script:

| Role | Login | Password | One-time code |
|---|---|---|---|
| Student | `21/0345` | `Student@123` | Not required |
| Bursary Admin | `bursar@kingsgate.edu.ng` | `Admin@123` | `123456` |
| Super Admin | `sysadmin@kingsgate.edu.ng` | `Super@123` | `123456` |

The sample environment enables the demo one-time code for local testing. Email and SMS delivery are simulated in development; messages and verification details are logged or shown in the app rather than sent.

## Configuration and Database

The server reads environment variables from `server/.env`. The setup script creates this file from `server/.env.example` if it does not exist.

| Variable | Purpose |
|---|---|
| `NODE_ENV` | Runtime environment |
| `PORT` | API listening port (defaults to `4000`) |
| `DATABASE_URL` | Prisma database connection; defaults to local SQLite |
| `ALLOWED_ORIGINS` | Allowed browser origins for API requests |
| `BCRYPT_ROUNDS` | Password hashing work factor |
| `DEMO_OTP` | Optional fixed one-time code for demonstrations |
| `SESSION_IDLE_MINUTES` | Session idle timeout |
| `COOKIE_SECURE` | Whether session cookies require HTTPS |

For a production deployment, configure a persistent database and production secrets through the hosting provider's environment settings. The included SQLite configuration is intended for local development and is not suitable for durable data in an ephemeral serverless environment. To use PostgreSQL, change the Prisma datasource provider in `server/prisma/schema.prisma`, set `DATABASE_URL` to the PostgreSQL connection string, then run `npm run db:setup` from `server/`.

## Deployment

The project is deployed at [kingsgate-beta.vercel.app](https://kingsgate-beta.vercel.app/). The root `vercel.json` configures the Vite frontend and Express server as Vercel services, routes `/api/*` requests to the server, and sends other paths to the frontend for client-side routing.

The payment gateway is a simulation, and local development uses SQLite. A live, persistent service requires production database configuration and any needed provider credentials to be set in Vercel; do not commit secrets to the repository.

## Tests and Evidence

Run the API tests from the project root:

```bash
npm test
```

The test suite covers authentication, validation, access controls, payments, and other API behavior using a separate test database. Screenshots for the project's test evidence can be placed in `public/evidence/`; the Testing page can display them.

## Project Structure

```text
src/                     React application
  components/            Shared interface components
  context/               Authentication, data, theme, and toast state
  layouts/               Public and signed-in page layouts
  pages/
    admin/               Administrative pages
    public/              Public, authentication, and showcase pages
    student/             Student portal pages
server/
  prisma/                Database schema and seed data
  scripts/               Database setup
  src/
    routes/               Authentication, payment, and admin API routes
    lib/                  Sessions, fees, gateway, audit, and utilities
    middleware/           Authentication, validation, and security
  tests/                  API tests
public/
  evidence/               Optional test evidence screenshots
```
