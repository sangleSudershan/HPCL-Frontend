# HP Petroleum Lucky Draw System

A full-stack web application for managing HP Petroleum's promotional lucky draw events across multiple petrol pumps.

---

## 🗂 Project Structure

```
hp-petroleum/
├── backend/          # Node.js + Express API
│   ├── models/       # MongoDB schemas
│   ├── routes/       # API endpoints
│   ├── middleware/   # JWT auth middleware
│   ├── server.js     # Entry point
│   └── .env.example  # Environment config template
└── frontend/         # React.js app
    └── src/
        ├── pages/    # Login, Dashboard, Event, Winners, Import
        ├── components/ # Navbar, WinnerPopup
        ├── context/  # AuthContext
        └── utils/    # Axios API client
```

---

## ⚙️ Prerequisites

- Node.js v16+
- MongoDB (local or MongoDB Atlas)
- npm or yarn

---

## 🚀 Setup & Installation

### 1. Backend Setup

```bash
cd backend
npm install

# Create your .env file
cp .env.example .env
# Edit .env and set your MONGODB_URI and JWT_SECRET
```

**Edit `backend/.env`:**
```
PORT=5000
MONGODB_URI=mongodb://localhost:27017/hp_petroleum
JWT_SECRET=your_very_secret_key_here
JWT_EXPIRES_IN=7d
FRONTEND_URL=http://localhost:3000
```

**Start backend:**
```bash
npm run dev    # Development (with nodemon)
npm start      # Production
```

---

### 2. Frontend Setup

```bash
cd frontend
npm install
npm start      # Development server at http://localhost:3000
```

**For production build:**
```bash
npm run build
```

---

## 👤 Creating Petrol Pump Accounts

Use the `/api/auth/register` endpoint to create pump accounts:

```bash
curl -X POST http://localhost:5000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "pumpName": "HP Pump - Nagpur Central",
    "email": "nagpur@hppetroleum.com",
    "password": "secure123"
  }'
```

Or use Postman/Thunder Client to POST to `/api/auth/register`.

---

## 📊 Excel Import Format

The Excel file must have these column headers (exact match):

| CustomerName | CustomerPhone | VehicleNumber | CouponCode |
|---|---|---|---|
| Rahul Sharma | 9876543210 | MH31AB1234 | AXD7283 |
| Priya Patel | 9123456789 | MH12CD5678 | BZP9123 |

- PumpName is auto-set from your login account
- Duplicate coupon codes (per pump) are automatically skipped
- Supports `.xlsx` and `.xls` files

---

## 🔌 API Reference

### Authentication
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/auth/register` | Create pump account |
| POST | `/api/auth/login` | Login, returns JWT |
| GET | `/api/auth/me` | Get current user |

### Coupons
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/coupons/import` | Upload Excel file |
| GET | `/api/coupons` | List coupons |
| DELETE | `/api/coupons` | Clear all coupons |

### Events
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/events` | All 22 events with status |
| GET | `/api/events/:num` | Single event details |
| POST | `/api/events/reveal/:num` | Reveal winner |
| POST | `/api/events/reset/:num` | Reset event |
| POST | `/api/events/set-start-date` | Set campaign start date |

### Winners
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/winners` | All winners for this pump |

---

## 🔐 Security Features

- JWT-based authentication (7-day expiry)
- All routes protected — each pump only accesses their own data
- Passwords hashed with bcrypt (12 rounds)
- Compound indexes prevent duplicate coupon codes per pump
- One winner per event per pump enforced at DB level

---

## 🎯 Event Unlock Logic

- Event 1 is open from Day 1 (campaign start date)
- Each subsequent event unlocks on the next calendar day
- Campaign start date is set automatically on account creation
- Can be manually adjusted via `/api/events/set-start-date`

---

## 🛠 Tech Stack

- **Frontend:** React 18, React Router v6, React Toastify, Axios
- **Backend:** Node.js, Express.js
- **Database:** MongoDB with Mongoose
- **Auth:** JWT + bcryptjs
- **File Upload:** Multer + SheetJS (xlsx)

---

## 🌐 Deployment

### MongoDB Atlas (Cloud)
Replace `MONGODB_URI` with your Atlas connection string:
```
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/hp_petroleum
```

### Environment Variables (Production)
```
NODE_ENV=production
PORT=5000
MONGODB_URI=<atlas_uri>
JWT_SECRET=<strong_random_secret>
FRONTEND_URL=https://your-frontend-domain.com
```
# HPCL
# HPCL-Frontend
