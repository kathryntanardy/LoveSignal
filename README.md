# LoveSignal

LoveSignal is a location-based dating app built for CMDF 2026. Discover people nearby, swipe to connect, and see where your matches were when you both liked each other — on a live map with a time-limited window to meet up.

## Features

- **Discover feed** — Browse nearby profiles sorted by distance. Swipe right to match or left to pass.
- **Match map** — When you match with someone, their last-known location appears on a Google Map. Matches expire after 30 minutes, encouraging real-world meetups.
- **Mutual matches** — Get notified when both users swipe right on each other.
- **Profile & preferences** — View and edit your profile, bio, photo, age range, gender preferences, and visibility settings.
- **Location tracking** — The app updates your location in the background so discovery stays proximity-based.
- **Daily limits** — Up to 5 new matches per day to keep interactions intentional.

## Tech Stack

| Layer    | Technologies |
|----------|--------------|
| Frontend | React 19, TypeScript, Vite, React Router, Google Maps (`@vis.gl/react-google-maps`) |
| Backend  | Node.js, Express 5, MongoDB, Mongoose |
| Auth     | Token-based sessions with bcrypt password hashing |

## Project Structure

```
LoveSignal/
├── backend/          # Express API, MongoDB models, seed script
│   ├── controllers/
│   ├── middleware/
│   ├── models/
│   ├── routes/
│   └── server.js
└── frontend/         # React SPA
    └── src/
        ├── components/
        ├── hooks/
        └── pages/
```

## Getting Started

### Prerequisites

- Node.js 18+
- MongoDB instance (local or Atlas)
- Google Maps API key (for the match map)

### Backend

```bash
cd backend
npm install
```

Create a `.env` file in `backend/`:

```env
MONGO_URI=mongodb://localhost:27017/lovesignal
PORT=3000
```

Start the server:

```bash
npm run dev
```

Seed the database with sample users (optional):

```bash
node seedUsers.js
```

Seeded accounts use the password `seedPassword123!`. See `backend/fakeUsers15.json` for sample emails.

### Frontend

```bash
cd frontend
npm install
```

Create a `.env` file in `frontend/`:

```env
VITE_API_URL=http://localhost:3000
VITE_GOOGLE_MAPS_API_KEY=your_google_maps_api_key
```

Start the dev server:

```bash
npm run dev
```

Open the URL shown in the terminal (typically `http://localhost:5173`) and log in or sign up.

## API Overview

Full endpoint documentation lives in [`backend/API_DESIGN.md`](backend/API_DESIGN.md). Key routes:

| Method | Path | Description |
|--------|------|-------------|
| `POST` | `/api/auth/signup` | Register a new user |
| `POST` | `/api/auth/login` | Log in and receive an auth token |
| `GET`  | `/api/users/me` | Get current user profile |
| `PATCH`| `/api/users/me` | Update profile |
| `GET`  | `/api/users/others` | Discover nearby users |
| `POST` | `/api/users/me/matches` | Record a swipe-right match |
| `GET`  | `/api/users/me/matches` | List active matches for the map |

Protected routes require an `Authorization: Bearer <token>` header.

## How It Works

1. **Sign up or log in** — Create an account with a location so you appear in nearby discovery.
2. **Swipe on the dashboard** — Right swipes create matches; left swipes are recorded as pings.
3. **View the match map** — Matched users show up as pins at the coordinates captured at match time.
4. **Meet within 30 minutes** — Match pins expire after 30 minutes, then are removed automatically.

## License

ISC
