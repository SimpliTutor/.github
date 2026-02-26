![SimpliTutor Dashboard Light](https://github.com/SimpliTutor/.github/blob/main/images/lightmode.png?raw=true#gh-light-mode-only)
![SimpliTutor Dashboard Dark](https://github.com/SimpliTutor/.github/blob/main/images/darkmode.png?raw=true#gh-dark-mode-only)

## SimpliTutor

SimpliTutor is an operations platform for tutoring businesses. Session scheduling, student management, billing, and analytics—all in one place.

- [x] Interactive calendar with double-booking prevention
- [x] Session lifecycle tracking with tutor verification workflows
- [x] Automated session reminders via scheduled emails
- [x] Parent invoicing with Stripe payment links
- [x] Monthly tutor payroll aggregation and tracking
- [x] Real-time sync across all connected clients
- [x] Role-based access (RBAC)
- [x] At-risk student detection based on inactivity thresholds
- [x] Session notes with parent email delivery

<p align="center">
  <img
    src="https://github.com/SimpliTutor/.github/blob/main/images/demo2.png?raw=true"
    alt="SimpliTutor Dashboard"
    style="
      border-radius: 16px;
      box-shadow: 0 20px 40px rgba(0,0,0,0.35);
      border: 1px solid rgba(255,255,255,0.08);
    "
  />
</p>
<p align="center">
  <img
    src="https://github.com/SimpliTutor/.github/blob/main/images/demo1.png?raw=true"
    alt="SimpliTutor Dashboard"
    style="
      border-radius: 16px;
      box-shadow: 0 20px 40px rgba(0,0,0,0.35);
      border: 1px solid rgba(255,255,255,0.08);
    "
  />
</p>

## Tech Stack

| Frontend                      | Backend                | Infrastructure            |
| :---------------------------- | :--------------------- | :------------------------ |
| React 19 + TypeScript         | Node.js + Express      | Supabase (PostgreSQL)     |
| Redux Toolkit + Redux Persist | Socket.IO              | Firebase Auth             |
| Tailwind CSS 4 + shadcn/ui    | Prisma ORM             | Stripe                    |
| React Hook Form + Zod         | Nodemailer + node-cron | Sentry (client + server)  |
| Recharts + TanStack Table     | Pino logging           | Firebase Hosting + Heroku |

---

## Architecture

```
┌─────────────────────────────────────────────────────────┐
│  Client (React SPA on Firebase Hosting)                 │
│                                                         │
│  Redux Store ◄── Socket.IO ◄──────────────────────┐     │
│       │                                           │     │
│       └── REST API (token + uid headers) ──────┐  │     │
└────────────────────────────────────────────────┼──┼─────┘
                                                 │  │
                                        HTTP     │  │  WebSocket
                                                 ▼  │
┌────────────────────────────────────────────────────────┐
│  Server (Express on Heroku)                            │
│                                                        │
│  Firebase Admin ──► authorize() middleware             │
│       (JWT verify + role check)        │               │
│                                        ▼               │
│  Route handlers ──► Prisma ORM ──► PostgreSQL          │
│                                   (Supabase)           │
│                                                        │
│  Sentry ◄── Pino logger ◄── all errors/requests        │
│  Nodemailer + node-cron ◄── email & reminders          │
│  Stripe webhooks ◄── payment events                    │
└────────────────────────────────────────────────────────┘
```

**Real-time sync**: Socket.IO broadcasts domain events (`students`, `sessions`, `users`, `subjects`, `settings`) to all connected clients. Each event triggers a scoped data refresh via `RefreshFactory`, so the UI stays in sync without a full reload.

**Authentication**: Firebase Auth issues JWTs on the client. Every API request includes the token and Firebase UID in headers. The server's `authorize()` middleware verifies the JWT with Firebase Admin SDK, looks up the user's role in Supabase, and rejects requests that don't match the required role — before any handler logic runs.

**Observability**: Pino handles structured JSON request logging on the server. Sentry captures unhandled exceptions on both client (with session replay) and server (with full request traces).

---

## API

private
