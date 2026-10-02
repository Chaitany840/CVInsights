<div align="center">

# CVInsights

**AI-powered resume analyzer and career path advisor**

[Live demo](https://cvinsights-1.onrender.com) · [Report a bug](https://github.com/Chaitany840/CVInsights/issues)

![Node.js](https://img.shields.io/badge/Node.js-Express-339933?logo=node.js&logoColor=white)
![React](https://img.shields.io/badge/React-19-61DAFB?logo=react&logoColor=black)
![TypeScript](https://img.shields.io/badge/TypeScript-5.9-3178C6?logo=typescript&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-Mongoose-47A248?logo=mongodb&logoColor=white)
![Gemini](https://img.shields.io/badge/Google-Gemini%202.5%20Flash-4285F4?logo=google&logoColor=white)

</div>

---

## Overview

Most resume checkers stop at a score. CVInsights goes one step further: it analyzes a resume, explains *why* it scored the way it did, finds the skill gaps, and turns them into a learning roadmap and role recommendations.

## Features

- **ATS score with a breakdown:** overall score and verdict, plus separate scores for format, content, skill match, keyword match, missing sections, experience and projects
- **Skill extraction** and **missing keyword detection** (technical, tools, soft skills, domain)
- **Strengths, critical issues and specific suggestions** tied to the uploaded resume
- **Career path recommendations** with a top-matching role and match percentage
- **Personalized learning path** based on the detected skill gaps
- **Accounts and history:** JWT authentication with bcrypt-hashed passwords
- **Resume formats:** PDF and image uploads

## How it works

```
Upload (PDF / image)  ->  Express API  ->  Gemini 2.5 Flash (structured JSON)
                                                  |
                         Server-side score calibration  <-+
                                                  |
                              MongoDB  ->  React dashboard
```

1. The user signs up or logs in and uploads a resume.
2. The file is sent to Gemini 2.5 Flash with a fixed prompt and `temperature: 0`, and the model returns a structured JSON analysis.
3. The server **validates and normalizes** the response, so every field has the expected type and scores stay in the 0–100 range.
4. The overall score is then **recalculated on the server**. It detects whether the resume is fresher, early-career or experienced, applies level-specific weights, and adjusts for missing sections, weak phrasing and the absence of measurable results.
5. The result is stored in MongoDB and shown on the dashboard.

### Engineering details

| Concern | Approach |
|---|---|
| Consistent results | The same file or text always returns the same analysis, via a SHA-256 content-hash cache |
| API cost and abuse | 100 requests per IP per 15 minutes on `/api`, plus a 15-second per-user cooldown on AI calls |
| Duplicate work | Concurrent identical requests share one in-flight call |
| Rate-limit resilience | Retries with backoff on `429` responses, honoring Google's suggested retry delay, with a distinct path for daily quota exhaustion |
| Malformed model output | Markdown fences are stripped and JSON is extracted before parsing |
| Auth | Bearer-token JWT middleware protects private routes |

## Tech stack

| Layer | Technology |
|---|---|
| Frontend | React 19, TypeScript, Vite, Tailwind CSS 4, shadcn/ui (Radix), React Router, Framer Motion |
| Backend | Node.js, Express 5 |
| Database | MongoDB with Mongoose |
| AI | Google Gemini 2.5 Flash (`@google/generative-ai`) |
| Security | JWT, bcryptjs, CORS, express-rate-limit |
| File handling | Multer, pdf-parse |

## Project structure

```
CVInsights/
├── client/                 # Vite + React + TypeScript frontend
│   └── src/
├── server/
│   ├── controllers/
│   ├── middleware/         # JWT auth
│   ├── models/             # Mongoose models
│   ├── routes/             # auth, resume, content
│   ├── utils/              # Gemini analysis, text extraction, AI rate-limit helpers
│   └── server.js
└── package.json
```

## Getting started

### Prerequisites

- Node.js 18 or later
- A MongoDB database (local or Atlas)
- A [Google Gemini API key](https://aistudio.google.com/apikey)

### 1. Clone and install

```bash
git clone https://github.com/Chaitany840/CVInsights.git
cd CVInsights
npm install
cd client && npm install && cd ..
```

### 2. Configure environment

Create a `.env` file in the project root:

```env
MONGO_URI=your_mongodb_connection_string
JWT_SECRET=a_long_random_string
GEMINI_API_KEY=your_gemini_api_key
PORT=5000
```

| Variable | Required | Description |
|---|---|---|
| `MONGO_URI` | Yes | MongoDB connection string |
| `JWT_SECRET` | Yes | Secret used to sign and verify tokens |
| `GEMINI_API_KEY` | Yes | Google Gemini API key |
| `PORT` | No | API port (default `5000`) |

### 3. Run

```bash
# API server (from the project root)
npm start

# Frontend (in a second terminal)
cd client
npm run dev
```

Check that the API is up: `GET /api/health` returns `{ "status": "ok" }`.

## API overview

| Route prefix | Purpose |
|---|---|
| `/api/auth` | Registration and login |
| `/api/resume` | Resume upload and analysis |
| `/api/content` | Application content |
| `/api/health` | Health check |

## Roadmap

- [ ] Target-job-description matching
- [ ] DOCX resume support
- [ ] Persistent cache (Redis) in place of the in-memory cache
- [ ] Automated tests and CI

## Author

**Chaitany Kumar**: B.Tech CSE (AI), NIET Greater Noida
[LinkedIn](https://linkedin.com/in/chaitany-kumar) · [GitHub](https://github.com/Chaitany840)
