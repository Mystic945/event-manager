# 🎉 Event Manager

A full-stack event management web application built with React, Node.js, Express, PostgreSQL, and Google OAuth 2.0. Users can create, browse, and register for events — with role-based access for organizers and admins.

---

## Table of Contents

- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
  - [1. Clone the repository](#1-clone-the-repository)
  - [2. Set up environment variables](#2-set-up-environment-variables)
  - [3. Set up the database](#3-set-up-the-database)
  - [4. Run the backend](#4-run-the-backend)
  - [5. Run the frontend](#5-run-the-frontend)
- [Google OAuth Setup](#google-oauth-setup)
- [API Reference](#api-reference)
- [Database Schema](#database-schema)
- [Security](#security)
- [Deployment](#deployment)
- [Contributing](#contributing)
- [License](#license)

---

## Features

- **Google OAuth 2.0** — one-click sign-in, no passwords to manage
- **Event CRUD** — create, edit, publish, and delete events with image uploads
- **Event registration** — users can register and cancel their spot
- **Capacity management** — automatic waitlisting when events are full
- **Role-based access control** — `USER`, `ORGANIZER`, and `ADMIN` roles
- **Email notifications** — confirmation emails on registration via Nodemailer
- **Dashboard** — organizers see attendee lists; users see their upcoming events
- **Search & filter** — browse events by date, location, and category
- **Responsive UI** — works on mobile, tablet, and desktop

---

## Tech Stack

### Frontend
| Technology | Purpose |
|---|---|
| React 18 + Vite + TypeScript | UI framework and build tool |
| Tailwind CSS | Utility-first styling |
| React Router v6 | Client-side routing and protected routes |
| TanStack React Query | Server state management and caching |
| React Hook Form + Zod | Forms and schema validation |
| Axios | HTTP client with interceptors |

### Backend
| Technology | Purpose |
|---|---|
| Node.js + Express | REST API server |
| Passport.js | Google OAuth 2.0 authentication |
| Prisma ORM | Type-safe database access and migrations |
| Redis | Session storage and rate-limit state |
| Nodemailer | Transactional email notifications |
| Multer + Cloudinary | Image upload and CDN delivery |
| Helmet + express-rate-limit | HTTP security headers and rate limiting |

### Data & Infrastructure
| Technology | Purpose |
|---|---|
| PostgreSQL | Primary relational database |
| Redis | Session cache |
| Cloudinary | Image storage and CDN |
| Google Cloud Console | OAuth 2.0 identity provider |

---

## Project Structure

```
event-manager/
├── client/                        # React frontend (Vite)
│   ├── public/
│   ├── src/
│   │   ├── api/                   # Axios instance + resource helpers
│   │   │   ├── axios.ts
│   │   │   ├── events.ts
│   │   │   └── users.ts
│   │   ├── components/            # Reusable UI components
│   │   │   ├── EventCard.tsx
│   │   │   ├── Navbar.tsx
│   │   │   ├── Modal.tsx
│   │   │   └── Spinner.tsx
│   │   ├── context/
│   │   │   └── AuthContext.tsx    # Google OAuth session context
│   │   ├── hooks/
│   │   │   ├── useAuth.ts
│   │   │   └── useEvents.ts
│   │   ├── pages/
│   │   │   ├── Home.tsx
│   │   │   ├── Events.tsx
│   │   │   ├── EventDetail.tsx
│   │   │   ├── Dashboard.tsx
│   │   │   ├── CreateEvent.tsx
│   │   │   └── Login.tsx
│   │   ├── routes/
│   │   │   └── ProtectedRoute.tsx
│   │   ├── App.tsx
│   │   └── main.tsx
│   ├── .env.example
│   ├── index.html
│   ├── tailwind.config.ts
│   └── vite.config.ts
│
├── server/                        # Node.js + Express backend
│   ├── prisma/
│   │   ├── schema.prisma
│   │   └── migrations/
│   ├── src/
│   │   ├── config/
│   │   │   ├── passport.ts        # Google OAuth strategy
│   │   │   ├── redis.ts
│   │   │   └── cloudinary.ts
│   │   ├── middleware/
│   │   │   ├── auth.ts            # requireAuth, requireRole
│   │   │   ├── errorHandler.ts
│   │   │   └── rateLimiter.ts
│   │   ├── routes/
│   │   │   ├── auth.ts
│   │   │   ├── events.ts
│   │   │   ├── users.ts
│   │   │   └── registrations.ts
│   │   ├── controllers/
│   │   │   ├── eventController.ts
│   │   │   ├── authController.ts
│   │   │   └── registrationController.ts
│   │   ├── services/
│   │   │   ├── eventService.ts
│   │   │   └── emailService.ts
│   │   ├── utils/
│   │   │   └── validators.ts
│   │   └── index.ts               # App entry point
│   ├── .env.example
│   └── tsconfig.json
│
├── .gitignore
└── README.md
```

---

## Prerequisites

Make sure the following are installed before you begin:

- [Node.js](https://nodejs.org/) v18+
- [npm](https://www.npmjs.com/) v9+ or [pnpm](https://pnpm.io/)
- [PostgreSQL](https://www.postgresql.org/) v14+
- [Redis](https://redis.io/) v7+
- A [Google Cloud Console](https://console.cloud.google.com/) project with OAuth 2.0 credentials
- A [Cloudinary](https://cloudinary.com/) account (free tier)

---

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/your-username/event-manager.git
cd event-manager
```

### 2. Set up environment variables

**Backend — copy and fill in `/server/.env`:**

```bash
cp server/.env.example server/.env
```

```env
# Server
PORT=5000
NODE_ENV=development

# Database
DATABASE_URL=postgresql://YOUR_USER:YOUR_PASSWORD@localhost:5432/event_manager

# Redis
REDIS_URL=redis://localhost:6379

# Session
SESSION_SECRET=replace-with-a-long-random-string-min-32-chars

# Google OAuth
GOOGLE_CLIENT_ID=your-google-client-id.apps.googleusercontent.com
GOOGLE_CLIENT_SECRET=your-google-client-secret

# Frontend URL (used for CORS and OAuth redirect)
CLIENT_URL=http://localhost:5173

# Cloudinary
CLOUDINARY_CLOUD_NAME=your-cloud-name
CLOUDINARY_API_KEY=your-api-key
CLOUDINARY_API_SECRET=your-api-secret

# Email (example using Gmail SMTP)
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your-email@gmail.com
SMTP_PASS=your-app-password
EMAIL_FROM=no-reply@eventmanager.com
```

**Frontend — copy and fill in `/client/.env`:**

```bash
cp client/.env.example client/.env
```

```env
VITE_API_URL=http://localhost:5000/api
```

### 3. Set up the database

Create the database, then run Prisma migrations:

```bash
# Create the database (if it doesn't exist)
psql -U postgres -c "CREATE DATABASE event_manager;"

# Install server dependencies
cd server
npm install

# Run migrations and generate Prisma client
npx prisma migrate dev --name init
npx prisma generate

# (Optional) Seed with sample data
npx prisma db seed
```

### 4. Run the backend

```bash
# From /server
npm run dev
```

The API will be available at `http://localhost:5000`.

### 5. Run the frontend

```bash
# From /client
npm install
npm run dev
```

The app will be available at `http://localhost:5173`.

---

## Google OAuth Setup

1. Go to [Google Cloud Console](https://console.cloud.google.com/) → **APIs & Services** → **Credentials**.
2. Click **Create Credentials** → **OAuth 2.0 Client ID**.
3. Set the application type to **Web application**.
4. Add the following to **Authorized redirect URIs**:
   ```
   http://localhost:5000/api/auth/google/callback
   ```
   For production, also add:
   ```
   https://your-backend-domain.com/api/auth/google/callback
   ```
5. Copy the **Client ID** and **Client Secret** into your `server/.env`.

---

## API Reference

### Auth

| Method | Endpoint | Description | Auth required |
|---|---|---|---|
| `GET` | `/api/auth/google` | Redirect to Google login | No |
| `GET` | `/api/auth/google/callback` | Google OAuth callback | No |
| `GET` | `/api/auth/me` | Get current user | Yes |
| `POST` | `/api/auth/logout` | Log out and destroy session | Yes |

### Events

| Method | Endpoint | Description | Auth required |
|---|---|---|---|
| `GET` | `/api/events` | List all published events | No |
| `GET` | `/api/events/:id` | Get a single event | No |
| `POST` | `/api/events` | Create a new event | Yes (ORGANIZER+) |
| `PUT` | `/api/events/:id` | Update an event | Yes (owner or ADMIN) |
| `DELETE` | `/api/events/:id` | Delete an event | Yes (owner or ADMIN) |
| `POST` | `/api/events/:id/publish` | Publish an event | Yes (owner or ADMIN) |

### Registrations

| Method | Endpoint | Description | Auth required |
|---|---|---|---|
| `POST` | `/api/events/:id/register` | Register for an event | Yes |
| `DELETE` | `/api/events/:id/register` | Cancel registration | Yes |
| `GET` | `/api/events/:id/attendees` | List attendees | Yes (organizer or ADMIN) |

### Users

| Method | Endpoint | Description | Auth required |
|---|---|---|---|
| `GET` | `/api/users/me/events` | Get current user's registered events | Yes |
| `PUT` | `/api/users/me` | Update profile | Yes |
| `GET` | `/api/users` | List all users | Yes (ADMIN only) |

---

## Database Schema

```
User
  id           UUID (PK)
  googleId     String (unique)
  email        String (unique)
  name         String
  avatar       String?
  role         USER | ORGANIZER | ADMIN
  createdAt    DateTime

Event
  id           UUID (PK)
  title        String
  description  String
  location     String
  startDate    DateTime
  endDate      DateTime
  capacity     Int
  imageUrl     String?
  isPublished  Boolean
  creatorId    UUID (FK → User)
  createdAt    DateTime

Registration
  id           UUID (PK)
  userId       UUID (FK → User)
  eventId      UUID (FK → Event)
  status       PENDING | CONFIRMED | CANCELLED
  createdAt    DateTime
  UNIQUE(userId, eventId)
```

---

## Security

This project applies defense-in-depth across all layers:

- **Helmet.js** — sets HTTP security headers (CSP, HSTS, X-Frame-Options) on every response
- **CORS** — only the configured `CLIENT_URL` origin is allowed; credentials require an exact origin match
- **Rate limiting** — auth routes: 10 requests / 15 min per IP; general API: 100 requests / 15 min, backed by Redis
- **Session security** — sessions stored server-side in Redis; cookies are `httpOnly`, `secure` in production, and `sameSite: lax`
- **Input validation** — all request bodies are validated with Zod/express-validator before reaching controllers
- **SQL injection prevention** — Prisma uses parameterized queries throughout; raw queries are never used
- **RBAC** — `requireRole()` middleware enforces permissions on every sensitive endpoint
- **Image validation** — Multer validates MIME type and file size before any upload reaches Cloudinary
- **Environment secrets** — `.env` files are gitignored; secrets are never committed or logged
- **Dependency auditing** — run `npm audit` regularly; keep dependencies up to date

---

## Deployment

### Recommended stack

| Layer | Service |
|---|---|
| Frontend | [Vercel](https://vercel.com) |
| Backend | [Railway](https://railway.app) or [Render](https://render.com) |
| Database | Railway PostgreSQL or [Supabase](https://supabase.com) |
| Redis | Railway Redis or [Upstash](https://upstash.com) |
| Images | [Cloudinary](https://cloudinary.com) |
| DNS + TLS | [Cloudflare](https://cloudflare.com) |

### Deploy the backend

```bash
# Build TypeScript
cd server
npm run build

# Set all production environment variables in your hosting dashboard
# then deploy the /server directory
```

### Deploy the frontend

```bash
cd client
npm run build
# Deploy the /client/dist directory to Vercel
```

Update the Google Cloud Console redirect URI to your production backend URL after deploying.

---

## Contributing

Pull requests are welcome. For major changes, please open an issue first to discuss what you'd like to change.

```bash
# Create a feature branch
git checkout -b feature/your-feature-name

# Make your changes and commit
git commit -m "feat: add your feature"

# Push and open a PR
git push origin feature/your-feature-name
```

Please make sure tests pass and the linter is clean before submitting a PR.

---

## License

[MIT](LICENSE)
