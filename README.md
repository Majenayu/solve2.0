# PlacementPro – Campus Career Suite

An integrated placement management web app for colleges — connecting TPO admins, students, and alumni on one platform.

🔗 **Live:** [solve-q7hx.onrender.com](https://solve-q7hx.onrender.com)  
📦 **Repo:** [github.com/Majenayu/Solve](https://github.com/Majenayu/Solve)

---

## Topics

- [Tech Stack](#tech-stack)
- [Getting Started](#getting-started)
- [Environment Variables](#environment-variables)
- [Pages](#pages)
- [API Routes](#api-routes)
- [Deploy](#deploy)

---

## Tech Stack

| | |
|---|---|
| Backend | Node.js + Express |
| Database | MongoDB Atlas |
| Auth | Google OAuth 2.0 + Sessions |
| AI | Gemini 2.0 Flash |
| Frontend | HTML / CSS / JS |
| Hosting | Render.com |

---

## Getting Started

```bash
git clone https://github.com/Majenayu/Solve.git
cd Solve
npm install
cp .env.example .env   # fill in your values
npm run dev            # http://localhost:3000
```

**Default admin login:** `username: a` / `password: a`

---

## Environment Variables

```env
MONGO_URI=mongodb+srv://...
SESSION_SECRET=your-secret
PORT=3000
GEMINI_API_KEY=your-key
```

---

## Pages

| URL | Description |
|-----|-------------|
| `/` | Login (Admin & Student) |
| `/dashboard` | Admin / TPO panel |
| `/student` | Student portal |
| `/alumni-login` | Alumni register / login |
| `/alumni` | Alumni dashboard |
| `/alumni-connect` | Alumni group connect |

---

## API Routes

**Auth** — `/api/auth/login` · `/api/auth/logout` · `/api/auth/google` · `/api/auth/google-alumni`

**Students** — `/api/students` · `/api/students/import` · `/api/students/eligible` · `/api/students/me/:usn`

**Drives** — `/api/drives` · `/api/drives/:id/notify` · `/api/drives/:id/shortlist` · `/api/drives/student/:usn`

**Assessments** — `/api/assessments` · `/api/assessments/:id/take` · `/api/assessments/:id/submit`

**Notifications** — `/api/notifications/:usn` · `/api/notifications/:id/read`

**Alumni** — `/api/alumni/me` · `/api/alumni/groups` · `/api/alumni/private` · `/api/alumni/list`

**AI** — `/api/quiz/generate` · `/api/chat` · `/api/resume/compare`

**Dashboard** — `/api/dashboard/stats`

---

## Deploy

Configured for Render via `render.yaml`. Push to GitHub — Render auto-deploys.

```bash
git add .
git commit -m "update"
git push origin main
```
