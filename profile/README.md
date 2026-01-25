![SimpliTutor Dashboard Light](https://github.com/bemtutoringconsulting/.github-private/blob/main/images/lightmode.png?raw=true#gh-light-mode-only)
![SimpliTutor Dashboard Dark](https://github.com/bemtutoringconsulting/.github-private/blob/main/images/darkmode.png#gh-dark-mode-only)


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
    src="https://github.com/bemtutoringconsulting/.github-private/blob/main/images/demo2.png?raw=true"
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
    src="https://github.com/bemtutoringconsulting/.github-private/blob/main/images/demo1.png?raw=true"
    alt="SimpliTutor Dashboard"
    style="
      border-radius: 16px;
      box-shadow: 0 20px 40px rgba(0,0,0,0.35);
      border: 1px solid rgba(255,255,255,0.08);
    "
  />
</p>

## Tech Stack

| Frontend | Backend | Database & Auth |
|----------|---------|-----------------|
| React 19 | Node.js 18+ | Supabase (PostgreSQL) |
| TypeScript | Express.js | Firebase Auth |
| Redux Toolkit | Socket.IO | Stripe |
| Tailwind CSS | Nodemailer | |
| shadcn/ui | | |

---

## Architecture

```
Client (React SPA)
       │
       ├── REST API + WebSocket
       ▼
Server (Express.js)
       │
       ├── Firebase Admin (JWT Verification)
       ├── Authorization Middleware
       ▼
Supabase (PostgreSQL)
```

**Real-time sync**: Socket.IO broadcasts database changes to all connected clients, triggering automatic UI updates across sessions.

**Authentication**: Firebase handles user auth, Express middleware verifies JWTs and resolves user roles from Supabase before allowing API access.

---

## API

private
