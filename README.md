# Local Notice Board Platform

A hyperlocal MERN app combining business advertisements and community notices —
built to fulfill the PRD's Phase 1 scope: auth, business ads, community notices,
location-based discovery, engagement (like/comment/share/save/report), and an
admin panel.

Architecture note: Ads and Notices share a single `Post` model (differentiated
by a `type` field), so there's one Discovery/Feed engine instead of two parallel
ones. See `/server/models/Post.js`.

---

Live deployed link --> https://notice-board-kappa-one.vercel.app/


---

## 1. Local Setup

### Backend
```bash
cd server
npm install
cp .env.example .env   # fill in MONGO_URI, JWT_SECRET, Cloudinary keys
npm run seed            # loads realistic demo data (admin login printed to console)
npm run dev              # starts on http://localhost:5000
```

### Frontend
```bash
cd client
npm install
cp .env.example .env   # VITE_API_URL should point at your backend
npm run dev              # starts on http://localhost:5173
```

You need a free [MongoDB Atlas](https://www.mongodb.com/cloud/atlas) cluster
(get `MONGO_URI` from there) and a free [Cloudinary](https://cloudinary.com/)
account (for `CLOUDINARY_*` keys, used for ad/notice images).

After seeding, log in as:
- **Admin:** `admin@noticeboard.local` / `admin1234`
- **Resident:** `ritika@example.com` / `password123`
- **Business owner:** `anil@primetailors.local` / `password123`

---

## 2. What's Implemented (maps to the PRD)

| PRD Module | Where |
|---|---|
| User registration & login | `/server/routes/authRoutes.js`, `/client/src/pages/Login.jsx`, `Register.jsx` |
| Business profile + ad CRUD | `businessRoutes.js`, `postController.js`, `/client/src/pages/BusinessProfile.jsx`, `CreatePost.jsx` |
| Community notice CRUD | `postController.js` (`type: 'notice'`), `CreatePost.jsx` |
| Location-based discovery, search, category filter | `feedRoutes.js` → `getFeed`, `/client/src/pages/Feed.jsx` |
| Engagement (like/comment/share/save/report) | `engagementController.js`, `/client/src/pages/PostDetail.jsx` |
| Admin: manage users, moderate, view reports, analytics | `adminController.js`, `/client/src/pages/AdminPanel.jsx` |
| Realistic seed data | `/server/utils/seed.js` |

Out of scope (per PRD Phase 1): paid ads, ad-targeting algorithms, native mobile app.

---

## 5. Project Structure
```
notice-board/
├── server/
│   ├── config/       (db.js, cloudinary.js)
│   ├── models/       (User, Business, Post, Report)
│   ├── controllers/  (auth, business, post, engagement, admin)
│   ├── routes/
│   ├── middleware/   (auth, upload, errorHandler)
│   ├── utils/seed.js
│   └── server.js
└── client/
    └── src/
        ├── api/axios.js
        ├── context/AuthContext.jsx
        ├── components/ (Navbar, PostCard, ProtectedRoute)
        ├── pages/ (Feed, Login, Register, PostDetail, CreatePost,
        │           BusinessProfile, AdminPanel, SavedPosts)
        └── App.jsx
```
