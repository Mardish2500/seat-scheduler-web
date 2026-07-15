<div align="center">

# 🎬 Seat Scheduler Web

### Full-Stack Movie Booking & Seat Reservation Platform

![React](https://img.shields.io/badge/React-19-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![Vite](https://img.shields.io/badge/Vite-Frontend-646CFF?style=for-the-badge&logo=vite&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-18%2B-339933?style=for-the-badge&logo=node.js&logoColor=white)
![Express](https://img.shields.io/badge/Express-5-000000?style=for-the-badge&logo=express&logoColor=white)
![Prisma](https://img.shields.io/badge/Prisma-ORM-2D3748?style=for-the-badge&logo=prisma&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-MariaDB-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![Tailwind](https://img.shields.io/badge/TailwindCSS-4-06B6D4?style=for-the-badge&logo=tailwindcss&logoColor=white)
![License](https://img.shields.io/badge/License-See%20LICENSE-green?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Production--Ready-success?style=for-the-badge)

</div>

A full-stack movie booking application for scheduling showtimes and managing seat reservations.

This repository is organized as a monorepo with a React/Vite frontend, an Express backend, and a Prisma schema for the database.

## Status

Production-ready with the following features:

- User authentication (register, login, JWT-based sessions)
- Movie browsing with search, filter, and sort
- Seat selection and booking system
- User profile management
- Admin dashboard for managing movies, schedules, and bookings
- Responsive UI with Tailwind CSS and DaisyUI

## Tech Stack

| Area        | Tools                                                   |
| ----------- | ------------------------------------------------------- |
| Frontend    | React 19, Vite, React Router, Tailwind CSS 4, DaisyUI 5 |
| Backend     | Node.js, Express 5, Prisma ORM                          |
| Database    | MySQL / MariaDB                                         |
| Auth        | JWT (jsonwebtoken), bcrypt                              |
| Testing     | Vitest, React Testing Library                           |
| Development | Nodemon, ESLint                                         |

## 🧠 System Flow

### Arsitektur Umum

```mermaid
flowchart LR
    subgraph Client["🖥️ Frontend (React + Vite)"]
        A[Pages & Components]
        B[Auth Context]
        C[API Client]
    end

    subgraph Server["⚙️ Backend (Express 5)"]
        D[Routes]
        E[Middlewares\nJWT Auth]
        F[Controllers]
        G[Models]
    end

    subgraph DB["🗄️ Database"]
        H[(MySQL / MariaDB)]
    end

    A --> C
    B --> C
    C -- HTTP/JSON --> D
    D --> E
    E --> F
    F --> G
    G -- Prisma ORM --> H
```

### Alur Autentikasi & Booking Kursi

```mermaid
sequenceDiagram
    participant U as 👤 User
    participant FE as 🖥️ Frontend
    participant BE as ⚙️ Backend API
    participant DB as 🗄️ Database

    U->>FE: Login (email & password)
    FE->>BE: POST /api/users/login
    BE->>DB: Verifikasi kredensial
    DB-->>BE: Data user valid
    BE-->>FE: JWT Token
    FE-->>U: Redirect ke Home

    U->>FE: Pilih film & jadwal
    FE->>BE: GET /api/schedules/:id/seats
    BE->>DB: Ambil status kursi
    DB-->>BE: Daftar kursi tersedia
    BE-->>FE: Data kursi

    U->>FE: Pilih kursi & konfirmasi
    FE->>BE: POST /api/bookings (dengan JWT)
    BE->>BE: Validasi token (authMiddleware)
    BE->>DB: Simpan booking baru
    DB-->>BE: Booking berhasil
    BE-->>FE: Status CONFIRMED
    FE-->>U: Tampilkan halaman konfirmasi
```

### Alur Peran Pengguna (User Role Flow)

```mermaid
flowchart TD
    A([Pengguna Mengakses Aplikasi]) --> B{Sudah Login?}
    B -- Belum --> C[Halaman Login/Register]
    C --> B
    B -- Sudah --> D{Role Pengguna}

    D -- USER --> E[Browse Film & Jadwal]
    E --> F[Pilih Kursi & Booking]
    F --> G[Kelola Profil & Riwayat Booking]

    D -- ADMIN --> H[Akses Admin Dashboard]
    H --> I[Kelola Film]
    H --> J[Kelola Jadwal]
    H --> K[Kelola & Pantau Semua Booking]
```

## Project Structure

```text
seat-scheduler-web/
├── backend/
│   ├── prisma/
│   │   ├── migrations/       # Database migration files
│   │   ├── schema.prisma     # Database schema
│   │   └── seed.js           # Seed data script
│   ├── src/
│   │   ├── controllers/      # Request handlers
│   │   │   ├── adminController.js
│   │   │   ├── bookingController.js
│   │   │   ├── movieController.js
│   │   │   ├── scheduleController.js
│   │   │   └── userController.js
│   │   ├── lib/              # Shared utilities
│   │   │   ├── apiResponse.js
│   │   │   ├── jwt.js
│   │   │   ├── prisma.js
│   │   │   ├── validateEnv.js
│   │   │   └── validation.js
│   │   ├── middlewares/      # Express middlewares
│   │   │   └── authMiddleware.js
│   │   ├── models/           # Database access layer
│   │   │   ├── bookingModel.js
│   │   │   ├── movieModel.js
│   │   │   ├── scheduleModel.js
│   │   │   └── userModel.js
│   │   ├── routes/           # API route definitions
│   │   │   ├── adminRoutes.js
│   │   │   ├── bookingRoutes.js
│   │   │   ├── movieRoutes.js
│   │   │   ├── scheduleRoutes.js
│   │   │   └── userRoutes.js
│   │   └── app.js            # Express app entry point
│   ├── __tests__/            # Backend unit tests
│   ├── .env.example
│   └── package.json
├── frontend/
│   ├── src/
│   │   ├── api/              # API client
│   │   │   └── client.js
│   │   ├── components/       # Reusable UI components
│   │   │   ├── AdminRoute.jsx
│   │   │   ├── ErrorBoundary.jsx
│   │   │   ├── Footer.jsx
│   │   │   ├── Navbar.jsx
│   │   │   ├── PageTransition.jsx
│   │   │   ├── Skeleton.jsx
│   │   │   └── Toast.jsx
│   │   ├── context/          # React context providers
│   │   │   ├── AuthContext.jsx
│   │   │   ├── BuyerQueueContext.jsx
│   │   │   └── UndoStackContext.jsx
│   │   ├── pages/            # Page components
│   │   │   ├── admin/        # Admin dashboard pages
│   │   │   │   ├── Bookings.jsx
│   │   │   │   ├── Movies.jsx
│   │   │   │   └── Schedules.jsx
│   │   │   ├── BookingConfirmation.jsx
│   │   │   ├── Home.jsx
│   │   │   ├── Login.jsx
│   │   │   ├── MyBookings.jsx
│   │   │   ├── Profile.jsx
│   │   │   ├── Register.jsx
│   │   │   ├── ScheduleDetail.jsx
│   │   │   └── SeatSelection.jsx
│   │   ├── __tests__/        # Frontend unit tests
│   │   ├── App.jsx           # Main app component with routes
│   │   ├── index.css         # Global styles
│   │   └── main.jsx          # React entry point
│   ├── public/
│   ├── .env.example
│   ├── index.html
│   ├── vite.config.js
│   └── package.json
├── LICENSE
├── PLAN.md                   # Development roadmap
└── README.md
```

## Prerequisites

- Node.js 18 or newer
- npm
- MySQL or MariaDB

## Environment Variables

### Backend (`backend/.env`)

Create `backend/.env` from the example file:

```bash
cd backend
cp .env.example .env
```

| Variable            | Required | Description                                                           |
| ------------------- | -------- | ----------------------------------------------------------------------|
| `PORT`              | No       | Server port (default: 3000)                                           |
| `JWT_SECRET`        | **Yes**  | Secret key for JWT tokens (min 32 characters)                         |
| `DATABASE_URL`      | **Yes**  | MySQL connection string (format: `mysql://user:pass@host:port/db`)    |
| `CORS_ORIGIN`       | No       | Comma-separated allowed origins. Leave empty to allow all (dev only). |
| `DATABASE_USER`     | No       | MySQL username                                                        |
| `DATABASE_PASSWORD` | No       | MySQL password                                                        |
| `DATABASE_NAME`     | No       | Database name                                                         |
| `DATABASE_HOST`     | No       | Database host (default: localhost)                                    |
| `DATABASE_PORT`     | No       | Database port (default: 3306)                                         |

### Frontend (`frontend/.env`)

Create `frontend/.env` from the example file:

```bash
cd frontend
cp .env.example .env
```

| Variable        | Required | Description                                                 |
| --------------- | -------- | ------------------------------------------------------------|
| `VITE_API_BASE` | No       | Backend API base URL (default: `http://localhost:3000/api`) |

## Installation

Install dependencies for each workspace:

```bash
# Install backend dependencies
cd backend
npm install

# Install frontend dependencies
cd ../frontend
npm install
```

## Running Locally

Start the backend:

```bash
cd backend
npm run dev
```

The backend defaults to `http://localhost:3000`.

Start the frontend in another terminal:

```bash
cd frontend
npm run dev
```

Vite will print the local frontend URL, usually `http://localhost:5173`.

## Database

The Prisma schema lives at `backend/prisma/schema.prisma`.

### Core Models

| Model      | Purpose                                                                 |
| ---------- | ------------------------------------------------------------------------|
| `User`     | Stores account credentials (username, email, password, role)           |
| `Movie`    | Stores movie metadata (title, description, duration, genre, posterUrl) |
| `Schedule` | Stores a movie showtime, studio, and ticket price                      |
| `Booking`  | Connects a user, schedule, and seat number with status                 |

### Useful Prisma Commands

Run from `backend/`:

```bash
# Run pending migrations
npx prisma migrate dev

# Generate Prisma client
npx prisma generate

# Open Prisma Studio (database GUI)
npx prisma studio

# Seed the database
npm run seed
```

## Seed Data

The seed script (`backend/prisma/seed.js`) populates the database with realistic test data:

| Entity    | Count | Details                                                                              |
| --------- | ----- | -------------------------------------------------------------------------------------|
| Users     | 56    | 3 admins + 53 regular users (all passwords: `password123`)                           |
| Movies    | 66    | Across 8 genres: Action, Sci-Fi, Comedy, Drama, Horror, Romance, Animation, Thriller |
| Schedules | ~120  | 2-3 per movie, varied showtimes, 10 studios, prices from 30,000 to 100,000           |
| Bookings  | ~250  | Mixed statuses: CONFIRMED, CANCELLED, PENDING                                        |

### Admin Accounts

| Email                    | Password      |
| ------------------------ | --------------|
| `admin@example.com`      | `password123` |
| `superadmin@example.com` | `password123` |
| `manager@example.com`    | `password123` |

### Sample User Accounts

| Email               | Password      |
| ------------------- | --------------|
| `alice@example.com` | `password123` |
| `bob@example.com`   | `password123` |
| `demo@example.com`  | `password123` |

Run the seed script:

```bash
cd backend
npm run seed
```

## Scripts

### Backend

| Command              | Description                             |
| -------------------- | -----------------------------------------|
| `npm run dev`        | Starts the Express server with Nodemon  |
| `npm start`          | Starts the Express server (production)  |
| `npm test`           | Runs unit tests with Vitest             |
| `npm run test:watch` | Runs unit tests in watch mode           |
| `npm run seed`       | Seeds the database with initial data    |

### Frontend

| Command              | Description                          |
| -------------------- | --------------------------------------|
| `npm run dev`        | Starts the Vite development server   |
| `npm run build`      | Builds the frontend for production   |
| `npm run preview`    | Serves the production build locally  |
| `npm run lint`       | Runs ESLint                          |
| `npm test`           | Runs unit tests with Vitest          |
| `npm run test:watch` | Runs unit tests in watch mode        |

## API

Base URL in development:

```text
http://localhost:3000
```

### Public Endpoints

| Method | Endpoint                   | Description                                  | Auth |
| ------ | --------------------------- | --------------------------------------------- | ---- |
| `GET`  | `/`                        | Health check                                 | No   |
| `GET`  | `/health/db`               | Database connection test                     | No   |
| `GET`  | `/api/movies`              | List all movies with schedules               | No   |
| `GET`  | `/api/movies/:id`          | Get a single movie                           | No   |
| `GET`  | `/api/schedules`           | List schedules (optional `?movieId=` filter) | No   |
| `GET`  | `/api/schedules/:id`       | Get a single schedule                        | No   |
| `GET`  | `/api/schedules/:id/seats` | Get seat availability for a schedule         | No   |
| `POST` | `/api/users/register`      | Register a new user                          | No   |
| `POST` | `/api/users/login`         | Login and get JWT token                      | No   |

### Authenticated Endpoints

| Method  | Endpoint                   | Description                  | Auth |
| ------- | --------------------------- | ------------------------------| ---- |
| `GET`   | `/api/users/me`            | Get current user profile     | Yes  |
| `PATCH` | `/api/users/me`            | Update username and email    | Yes  |
| `PATCH` | `/api/users/me/password`   | Change password              | Yes  |
| `GET`   | `/api/bookings`            | List current user's bookings | Yes  |
| `GET`   | `/api/bookings/:id`        | Get a single booking         | Yes  |
| `POST`  | `/api/bookings`            | Create a new booking         | Yes  |
| `PATCH` | `/api/bookings/:id/cancel` | Cancel a booking             | Yes  |

### Admin Endpoints

| Method   | Endpoint                   | Description                                    | Auth  |
| -------- | --------------------------- | ------------------------------------------------| ----- |
| `POST`   | `/api/movies`              | Create a movie                                 | Admin |
| `PATCH`  | `/api/movies/:id`          | Update a movie                                 | Admin |
| `DELETE` | `/api/movies/:id`          | Delete a movie                                 | Admin |
| `POST`   | `/api/schedules`           | Create a schedule                              | Admin |
| `DELETE` | `/api/admin/schedules/:id` | Delete a schedule                              | Admin |
| `GET`    | `/api/admin/bookings`      | List all bookings (optional `?status=` filter) | Admin |

## User Roles

The application supports two user roles:

- **USER**: Can browse movies, book seats, view their bookings, and manage their profile
- **ADMIN**: Has all USER permissions plus access to the admin dashboard for managing movies, schedules, and viewing all bookings

## Testing

The project includes comprehensive unit tests:

- **Backend**: 56 tests covering validation, auth middleware, and booking controller
- **Frontend**: 60 tests covering contexts (BuyerQueue, UndoStack), components (HighlightText), and utility functions (search/filter/sort)

Run tests:

```bash
# Backend tests
cd backend
npm test

# Frontend tests
cd frontend
npm test
```

## Deployment

The frontend is configured for GitHub Pages deployment:

```bash
cd frontend
npm run deploy
```

## License

This project is licensed under the terms in [LICENSE](./LICENSE).

---

<div align="center">

## 👤 Author

**Mardiansyah**
Program Studi Teknik Informatika — Universitas Teknologi Bandung

⭐ *Jika proyek ini bermanfaat, jangan lupa beri bintang pada repository ini!* ⭐

</div>
