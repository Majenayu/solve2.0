# PlacementPro – Integrated Campus Career Suite

> A full-stack web application for managing college placement activities — built for TPO admins, students, and alumni.

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Environment Variables](#environment-variables)
- [Pages & Routes](#pages--routes)
- [API Reference](#api-reference)
- [Deployment](#deployment)
- [PWA Support](#pwa-support)

---

## Overview

PlacementPro is a campus placement management system that connects three user groups:

| Role | Access | Description |
|------|--------|-------------|
| **Admin (TPO)** | `/dashboard` | Manage students, drives, assessments, shortlisting |
| **Student** | `/student` | View eligible drives, take assessments, get notifications |
| **Alumni** | `/alumni` | Connect with students, join company groups, share resources |

---

## Features

### Admin (TPO)
- Dashboard with live stats — total students, drives, shortlisted count, assessments
- Student database — add, delete, search, filter by branch, import via CSV
- Placement drives — create with CGPA/backlog/branch/year eligibility filters
- One-click notify eligible students and shortlist with ranked results
- Assessment builder — create MCQ tests with timer, link to drives
- Assignment monitor — track student submissions and attempt analytics
- Interview scheduler — manage slots for shortlisted students
- Alumni group management

### Student
- Personalised dashboard — eligible drives, available tests, shortlist status
- Take timed assessments with auto-submit
- View placement drive details
- Real-time notifications with unread badge
- Resume wizard with AI-powered feedback
- AI mock interview and quiz generation (Gemini 2.0 Flash)
- Course recommendations feed

### Alumni
- Google OAuth login with profile registration
- Create and join company-specific groups
- Group chat with resource sharing
- Direct messaging between alumni
- Profile management (company, designation, LinkedIn, GitHub)
- Connect page for students to browse alumni

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| **Backend** | Node.js, Express.js |
| **Database** | MongoDB Atlas (Mongoose ODM) |
| **Authentication** | Express Session + Google OAuth 2.0 |
| **AI** | Google Gemini 2.0 Flash (primary), OpenRouter (fallback) |
| **File Uploads** | Multer |
| **CSV Import** | csv-parser |
| **Frontend** | Vanilla HTML/CSS/JS (no framework) |
| **Deployment** | Render.com |

---

## Project Structure

```
placementpro/
├── server.js              # Main Express server + all API routes
├── package.json           # Dependencies and scripts
├── render.yaml            # Render.com deployment config
├── .env                   # Environment variables (not committed)
│
├── index.html             # Login page (Admin + Student) — PWA entry point
├── index1.html            # Alumni login & registration
├── dashboard.html         # Admin / TPO dashboard
├── dashboard1.html        # Student dashboard
├── dashboard-alumni.html  # Alumni dashboard
└── alumni-connect.html    # Alumni group connect page
```

---

## Getting Started

### Prerequisites

- Node.js v18 or higher
- A MongoDB Atlas cluster
- A Google Cloud project with OAuth 2.0 credentials
- A Gemini API key

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/Majenayu/Solve.git
cd Solve

# 2. Install dependencies
npm install

# 3. Create your environment file
cp .env.example .env
# Fill in the values (see Environment Variables below)

# 4. Start the development server
npm run dev

# 5. Open in browser
# http://localhost:3000
```

### Default Admin Login

```
Username: a
Password: a
```

> Change this in `server.js` before deploying to production.

---

## Environment Variables

Create a `.env` file in the project root:

```env
# MongoDB connection string
MONGO_URI=mongodb+srv://<user>:<password>@<cluster>.mongodb.net/placementpro

# Session secret — use a long random string in production
SESSION_SECRET=your-secret-key-here

# Server port
PORT=3000

# Google Gemini API key
GEMINI_API_KEY=your-gemini-api-key

# (Optional) OpenRouter fallback key
OPENROUTER_KEY=your-openrouter-key
```

> **Never commit your `.env` file.** It is already listed in `.gitignore`.

---

## Pages & Routes

| URL | File Served | Who Can Access |
|-----|-------------|----------------|
| `/` | `index.html` | Everyone (Login) |
| `/dashboard` | `dashboard.html` | Admin (TPO) |
| `/student` | `dashboard1.html` | Students |
| `/alumni-login` | `index1.html` | Alumni (Login/Register) |
| `/alumni` | `dashboard-alumni.html` | Alumni |
| `/alumni-connect` | `alumni-connect.html` | Alumni |

---

## API Reference

### Auth

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/auth/login` | Admin or student login |
| `POST` | `/api/auth/logout` | Destroy session |
| `GET` | `/api/auth/me` | Get current session user |
| `POST` | `/api/auth/google` | Student Google OAuth login |
| `POST` | `/api/auth/google-alumni` | Alumni Google OAuth login/register |
| `GET` | `/api/auth/profile-status/:usn` | Check if student profile is complete |

### Students

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/students` | Get all students |
| `POST` | `/api/students` | Add a student |
| `PUT` | `/api/students/:id` | Update a student |
| `DELETE` | `/api/students/:id` | Delete a student |
| `POST` | `/api/students/import` | Import students from CSV file |
| `POST` | `/api/students/eligible` | Get eligible students for given criteria |
| `GET` | `/api/students/me/:usn` | Get student profile by USN |

### Placement Drives

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/drives` | Get all drives |
| `POST` | `/api/drives` | Create a new drive |
| `PUT` | `/api/drives/:id` | Update drive (e.g. change status) |
| `DELETE` | `/api/drives/:id` | Delete a drive |
| `POST` | `/api/drives/:id/notify` | Notify all eligible students |
| `POST` | `/api/drives/:id/shortlist` | Shortlist and rank eligible students |
| `GET` | `/api/drives/student/:usn` | Get drives a student is eligible for |

### Assessments

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/assessments` | Get all assessments |
| `POST` | `/api/assessments` | Create an assessment |
| `PUT` | `/api/assessments/:id/toggle` | Toggle active/inactive |
| `DELETE` | `/api/assessments/:id` | Delete an assessment |
| `GET` | `/api/assessments/:id/take` | Get assessment for a student to take |
| `POST` | `/api/assessments/:id/submit` | Submit student answers |

### Notifications

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/notifications/:usn` | Get all notifications for a student |
| `PUT` | `/api/notifications/:id/read` | Mark one notification as read |
| `PUT` | `/api/notifications/markall/:usn` | Mark all as read |

### Dashboard

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/dashboard/stats` | Summary stats for admin overview |

### Alumni

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/alumni/me` | Get current alumni profile |
| `PUT` | `/api/alumni/me` | Update alumni profile |
| `GET` | `/api/alumni/list` | Get all alumni (for student connect page) |
| `GET` | `/api/alumni/groups` | Get all groups |
| `POST` | `/api/alumni/groups` | Create a group |
| `POST` | `/api/alumni/groups/:id/join` | Join a group |
| `DELETE` | `/api/alumni/groups/:id` | Delete a group |
| `GET` | `/api/alumni/groups/:id/members` | Get group members |
| `GET` | `/api/alumni/groups/:id/messages/:section` | Get group messages |
| `POST` | `/api/alumni/groups/:id/messages` | Post a message to a group |
| `GET` | `/api/alumni/private/:groupId/:alumniId` | Get DMs |
| `POST` | `/api/alumni/private` | Send a DM |

### AI Features

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/quiz/generate` | Generate AI quiz questions (Gemini) |
| `POST` | `/api/chat` | AI mock interview / chat |
| `POST` | `/api/resume/compare` | AI resume feedback vs job description |

---

## Deployment

The app is configured for **Render.com** via `render.yaml`.

### Deploy on Render

1. Push your code to GitHub
2. Go to [render.com](https://render.com) → **New Web Service**
3. Connect your GitHub repo (`Majenayu/Solve`)
4. Render will auto-detect `render.yaml` and configure:
   - **Build command:** `npm install`
   - **Start command:** `node server.js`
   - **Port:** `10000`
5. Set your environment variables in the Render dashboard under **Environment**
6. Click **Deploy**

### Manual Deploy Commands

```bash
# Push latest changes to trigger auto-deploy
git add .
git commit -m "your message"
git push origin main
```

---

## PWA Support

PlacementPro is a **Progressive Web App** — users can install it directly to their home screen.

- On **Android / Chrome / Edge**: An install banner appears automatically after 3 seconds
- On **iPhone / iPad (Safari)**: A step-by-step hint guides users to use *Share → Add to Home Screen*
- The app icon uses the **Training & Placement Cell** logo
- A **Service Worker** caches the app shell for offline access
- Launches in standalone mode (no browser UI) once installed

---

## License

ISC — see `package.json` for details.

---

*Built for VVCE — Vidyavardhaka College of Engineering, Mysuru*
