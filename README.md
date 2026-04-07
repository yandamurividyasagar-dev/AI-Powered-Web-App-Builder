# NxtBuild — AI-Powered Web App Builder

> Describe what you want. Get a working web app in seconds.

🚀 **[Live Demo → ai-powered-web-app-builder-client.onrender.com](https://ai-powered-web-app-builder-client.onrender.com)**

https://github.com/user-attachments/assets/3908e4e5-5988-4c39-8769-47da5900e2ed

---

## What is this?

Most people have ideas for web apps — portfolios, dashboards, landing pages — but building one from scratch takes hours even for experienced developers. NxtBuild closes that gap.

You type a description in plain English. NxtBuild uses Google Gemini AI to generate a complete, working HTML/CSS/JavaScript application and renders it live in the browser. You can keep refining it conversationally — "make the header blue", "add a contact form" — and the app updates in real time.



## Features

- **Conversational UI** — chat-style interface where you describe and iterate on your app naturally
- **Live Preview** — generated code renders instantly inside a sandboxed iframe
- **Code Editor** — view and manually edit the generated source code
- **Version History** — every generation is saved so you can track how the app evolved
- **Project Management** — create, rename, and delete multiple projects from your dashboard
- **Download** — export your app as a standalone HTML file, no lock-in
- **JWT Authentication** — secure register/login with bcrypt password hashing and 7-day token expiry

---

## Tech Stack

**Frontend**
- React 19 + React Router v7
- Vite (build tool and dev server)
- Axios for API requests
- js-cookie for token management

**Backend**
- Node.js + Express 5
- MongoDB + Mongoose
- bcryptjs for password hashing
- jsonwebtoken for auth tokens
- Google Gemini AI (`@google/genai`) — model: `gemini-2.5-flash`

---

## Project Structure

```
nxtbuild/
├── client/                   # React frontend
│   └── src/
│       ├── components/       # Navbar, ChatInput, ChatMessage, LivePreview, CodeEditor
│       ├── context/          # AuthContext, ToastContext
│       ├── pages/            # LandingPage, LoginPage, DashboardPage, BuilderPage
│       └── services/         # api.js, authService, projectService, generationService
│
└── server/                   # Express backend
    └── src/
        ├── config/           # gemini.config.js
        ├── constants/        # prompts.js (system prompt + builder)
        ├── controllers/      # auth, project, generation
        ├── middleware/        # auth.middleware.js (JWT verification)
        ├── models/           # User.model.js, Project.model.js
        ├── routes/           # auth, projects, generate + index aggregator
        ├── services/         # auth, project, gemini, generation
        └── utils/            # jwt.utils.js, code.utils.js (response parser)
```

---

## Getting Started

### Prerequisites

- Node.js (v18+)
- MongoDB Atlas account
- Google Gemini API key

### 1. Clone the repo

```bash
git clone https://github.com/yourusername/nxtbuild.git
cd nxtbuild
```

### 2. Set up environment variables

```bash
cd server
cp .env.example .env
```

Open `server/.env` and fill in:

```env
MONGODB_URI=your_mongodb_atlas_connection_string
JWT_SECRET=any_random_secret_string
GEMINI_API_KEY=your_gemini_api_key
```

### 3. Install dependencies

```bash
# Backend
cd server
npm install

# Frontend
cd ../client
npm install
```

### 4. Run the app

```bash
# In one terminal — start the backend
cd server
npm run dev

# In another terminal — start the frontend
cd client
npm run dev
```

Open [http://localhost:5173](http://localhost:5173) in your browser.

---

## API Overview

```
POST   /api/auth/register         Register a new account
POST   /api/auth/login            Login and receive a JWT
GET    /api/auth/me               Get current user profile (auth required)

GET    /api/projects              List all user projects
POST   /api/projects              Create a new project
GET    /api/projects/:id          Get project with full chat history
PUT    /api/projects/:id          Rename / update a project
DELETE /api/projects/:id          Delete a project

POST   /api/generate/:projectId   Generate code from a prompt (auth required)
```

---

## How Code Generation Works

1. User sends a prompt from the chat interface
2. The server builds a full context prompt — system instructions + last 10 messages + current code (if iterating)
3. This goes to Gemini (`gemini-2.5-flash`)
4. The response is parsed to separate the description text from the ```` ```html ``` ```` code block
5. The new message and code are saved to MongoDB (old code archived as a version)
6. The client receives the result and re-renders the live preview

---

## Deployment

The app is deployed on **Render** (both frontend and backend).

| Service | URL |
|---------|-----|
| Frontend | [ai-powered-web-app-builder-client.onrender.com](https://ai-powered-web-app-builder-client.onrender.com) |
| Backend | Render (Node.js web service) |

> **Note:** Free tier on Render spins down after inactivity — first load may take ~30 seconds to wake up.

---

## What I'd add next

- Version restore — let users roll back to any previous generated version
- Public share links — shareable URLs so others can view an app without logging in
- Prompt templates — pre-built starting points for common app types (portfolio, landing page, to-do list)
- Export to ZIP — download with separate HTML, CSS, and JS files

---

## Inspired by

[Bolt.new](https://bolt.new) · [Lovable.dev](https://lovable.dev) · [v0.dev](https://v0.dev) · [Replit Agent](https://replit.com)
