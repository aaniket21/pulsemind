# 🧠 PulseMind AI — Mood Detection System

[![Live Demo](https://img.shields.io/badge/Live_Demo-Hosted_on_Vercel-success?style=for-the-badge)](https://pulsemind.vercel.app/)

**Live Link:** [https://pulsemind.vercel.app/](https://pulsemind.vercel.app/)

PulseMind AI is a full-stack, AI-driven digital mental health and psychological support platform built specifically for university students. It transforms traditional passive wellness tools into an intelligent, gamified, and emotionally adaptive experience—meeting students where they are: their phones, laptops, and late-night study sessions.

---

## 📖 Table of Contents

1. [What This Project Does](#what-this-project-does)
2. [Live Hosting & Deployment](#live-hosting--deployment)
3. [Key Features](#key-features)
4. [Tech Stack](#tech-stack)
5. [System Architecture](#system-architecture)
6. [Repository Structure](#repository-structure)
7. [API Reference](#api-reference)
8. [Run Locally](#run-locally)
9. [Troubleshooting & Security](#troubleshooting--security)

---

## 🎯 What This Project Does

The platform aims to reduce student stress and improve resilience by providing:
- **AI Mood Detection** — Webcam-based emotion analysis with FER + fallback heuristics.
- **Gamified Therapy** — Therapeutic mini-games based on CBT, ACT, and resilience principles.
- **Personalized Recommendations** — Logistic-regression + confidence-weighted engine that adapts over time.
- **Psychological Analytics Dashboard** — Weekly/monthly mood trends, stress scoring, resilience tracking.
- **Peer Community Layer** — Moderated, stigma-free social support.
- **Gamification System** — Points, badges, streaks to incentivize daily engagement.
- **Realtime Alerts** — WebSocket-based live mood updates and admin notifications.

Every design decision is grounded in clinical frameworks: Cognitive Behavioural Therapy (CBT), Acceptance and Commitment Therapy (ACT), and Resilience Theory.

---

## 🌐 Live Hosting & Deployment

The application is fully deployed and accessible globally.

- **Frontend Hosting:** [Vercel](https://vercel.com/)
- **Backend Hosting:** Fly.io (API server)
- **Database:** MongoDB Atlas (Cloud Database)
- **Live URL:** [https://pulsemind.vercel.app/](https://pulsemind.vercel.app/)

### How it is hosted:
1. **Frontend**: The React + Vite SPA is deployed on **Vercel**, providing edge caching, CI/CD integration, and high availability. Vercel automatically builds and serves the optimized frontend assets.
2. **Backend**: The FastAPI Python application is containerized and hosted on **Fly.io**, which automatically scales and manages application availability close to users globally.
3. **Database**: A serverless **MongoDB Atlas** cluster stores user data, mood logs, journals, and community threads securely with network isolation.

---

## ✨ Key Features

### 1) Authentication & User Management
- Secure registration and login with JWT and bcrypt password hashing.
- Role-based access control (Student, Counsellor, Admin).
- Sensitive profile fields encrypted via Fernet symmetric encryption.

### 2) Mood Detection (Core AI Feature)
- Local BlazeFace model runs in-browser for privacy-first face detection.
- Captured frame sent to backend `/api/analyze-mood` endpoint.
- OpenCV cascade preprocessing → FER deep learning inference.
- Groups emotions into: Happy, Neutral, Sad, returning a confidence score.

### 3) AI Recommendation Engine
- Analyzes user's recent mood records and confidence history.
- Returns personalized activities, journaling prompts, breathing exercises, and games.

### 4) Gamification & Therapeutic Mini-Games
- Points engine, milestone badges, and streak counters for engagement.
- Mindfulness breathing, cognitive puzzles, and creative sandbox games.
- Post-game reflection surveys feeding into the AI engine.

### 5) Peer Community Layer
- Anonymous forum for students to post to moderated topic threads (Academic Stress, Self-Care, etc.).
- Automated crisis keyword filtering triggering counsellor alerts.

### 6) Analytics Dashboard
- Weekly and monthly mood trend charts and stress score calculation.
- Linear projection predicting mood direction for the next 3 days.

---

## 🛠️ Tech Stack

### Frontend
- **React 18 + Vite** for blazing fast performance.
- **Tailwind CSS** for custom design tokens and styling.
- **Zustand** for state management.
- **Recharts & Chart.js** for analytics visualization.
- **React-Webcam** & **BlazeFace** (TensorFlow.js) for in-browser face detection.

### Backend
- **FastAPI** + **Uvicorn** (Python 3.11).
- **MongoDB** + **Motor** (async driver).
- **JWT** (python-jose) + **Passlib** (bcrypt) for Auth.
- **SlowAPI** for rate limiting.
- **Fernet (Cryptography)** for sensitive data encryption.

### AI / ML
- **OpenCV** + **FER** (Facial Expression Recognition).
- **Scikit-learn** + **Numpy** for logistic regression models.

---

## 🏗️ System Architecture

```text
┌─────────────────────────────────────────────────────────┐
│                      CLIENT LAYER                       │
│   React + Vite SPA (Tailwind, Zustand, Axios, Recharts) │
│   BlazeFace (In-Browser Face Detection)                 │
│   WebSocket Client (per-user channel)                   │
└────────────────────────┬────────────────────────────────┘
                         │ HTTPS / WSS
┌────────────────────────▼────────────────────────────────┐
│                    API GATEWAY LAYER                    │
│   FastAPI (Python 3.11)                                 │
│   Routers: auth | mood | recommend | analytics |        │
│            notifications | websocket | games | journal  │
└──────┬───────────────┬──────────────────┬───────────────┘
       │               │                  │
┌──────▼──────┐ ┌──────▼──────┐ ┌────────▼──────────────┐
│  AI MODULE  │ │  SERVICES   │ │    DATA LAYER         │
│  detector.py│ │ gamification│ │  MongoDB Atlas        │
│  FER + CV2  │ │ analytics   │ │  Collections: users,  │
│  trend_pred │ │ recommend   │ │  moods, posts, etc.   │
└─────────────┘ └─────────────┘ └────────────────────────┘
```

---

## 📁 Repository Structure

```text
.
├── ai_module/                  # Emotion detection module dependencies
├── backend/
│   ├── app/
│   │   ├── api/routes/         # Auth, mood, analytics, recommendations, community, games
│   │   ├── core/               # Config, DB, crypto, security, rate limiter
│   │   ├── schemas/            # Pydantic models
│   │   └── services/           # Business logic & AI orchestration
│   ├── scripts/                # DB seeding, CI smoke tests
│   └── tests/                  # Pytest unit tests
├── frontend/
│   ├── src/
│   │   ├── pages/              # UI Views (Dashboard, Mood Detection, Community, etc.)
│   │   ├── components/         # Reusable UI elements (GlassCard, Buttons, etc.)
│   │   ├── services/           # Axios API wrappers
│   │   └── store/              # Zustand global state
│   └── package.json
└── docs/                       # Architecture, PRD, Technical docs
```

---

## 🔌 API Reference

**Base backend URL:** `http://localhost:8000/api` (Local) / Fly.io API URL (Prod)

| Module | Endpoint | Method | Description |
|---|---|---|---|
| **System** | `/health`, `/ready` | `GET` | Probes for liveness and database readiness |
| **Auth** | `/auth/register`, `/auth/login`, `/auth/me` | `POST` / `GET` | User registration, authentication, and profile |
| **Mood** | `/analyze-mood`, `/save-mood`, `/get-history` | `POST` / `GET` | Emotion analysis and mood logging |
| **AI** | `/recommend`, `/analytics/summary` | `GET` | Personalized recommendations & trend analytics |
| **Community**| `/community/posts`, `/community/threads` | `GET` / `POST` | Peer forum threads and posting |
| **Games** | `/games/feedback` | `POST` | Submit post-game reflection survey |
| **Realtime** | `/ws/mood/{user_id}` | `WS` | Per-user WebSocket channel for live updates |

*(Full interactive Swagger UI available at `/docs` when running locally)*

---

## 🚀 Run Locally

### Prerequisites
- **Node.js 20+**
- **Python 3.11+**
- **Docker** (Recommended) or **MongoDB** (Local instance)

### 1. Using Docker (Recommended)
This runs the entire stack (Frontend, Backend, MongoDB) in containers.

```bash
# Set up environment variables
cp backend/.env.example backend/.env
cp frontend/.env.example frontend/.env

# Build and start services
docker compose up --build
```
- Frontend: `http://localhost:5173`
- Backend API Docs: `http://localhost:8000/docs`

### 2. Manual Setup (Without Docker)

**Backend:**
```bash
cd backend
python -m venv .venv
source .venv/bin/activate  # On Windows: .\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
cp .env.example .env
# Edit .env with your local MONGODB_URI
python -m uvicorn app.main:app --host 0.0.0.0 --port 8000
```

**Frontend:**
```bash
cd frontend
npm install
cp .env.example .env
npm run dev
```

---

## 🛡️ Troubleshooting & Security

- **Security Note:** Passwords are hashed with bcrypt. Sensitive profile data is encrypted at rest using Fernet. Ensure your `SECRET_KEY` and `ENCRYPTION_KEY` are strong and kept secret in production.
- **Webcam Issues:** Ensure your browser has granted camera permissions. The BlazeFace model runs locally in your browser to detect faces before sending frames to the backend.
- **WebSocket Drops:** If realtime notifications fail, check if your firewall allows WSS (WebSocket Secure) connections.

---
