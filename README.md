# CVInsights: AI-Powered Resume & Career Path Analyzer

**Live demo:** https://cvinsights-1.onrender.com

A full-stack web app that analyzes resumes with AI and turns the result into career guidance: an ATS score, skill gaps and a step-by-step learning roadmap.

---

## Problem

Most resume checkers stop at a score. Users still don't know *what to learn next*. CVInsights closes that gap by combining resume analysis with career-path recommendations.

## Features

- Resume upload and text extraction (PDF)
- ATS score analysis
- Skill extraction from the resume
- Skill gap detection against a target career path
- Career path recommendations
- Personalized learning roadmap
- User accounts with JWT authentication
- Dashboard UI

## How it works

1. The user signs up or logs in (passwords hashed with bcrypt, sessions via JWT).
2. The resume is uploaded (Multer) and its text is extracted (pdf-parse).
3. The extracted text is sent to Google's Gemini API, which returns the ATS analysis, extracted skills, skill gaps and recommendations.
4. Results are stored in MongoDB (Mongoose) and shown on the dashboard.

## Tech stack

| Layer | Technology |
|---|---|
| Frontend | React, Tailwind CSS |
| Backend | Node.js, Express |
| Database | MongoDB (Mongoose) |
| AI | Google Generative AI (Gemini) |
| Auth & security | JWT, bcryptjs, CORS, express-rate-limit |
| File handling | Multer, pdf-parse |

## Getting started

```bash
git clone https://github.com/Chaitany840/CVInsights.git
cd CVInsights
npm install
```

Create a `.env` file in the project root with your MongoDB connection string, a JWT secret and a Gemini API key, then start the server.

## Author

Chaitany Kumar · [LinkedIn](https://linkedin.com/in/chaitany-kumar) · [GitHub](https://github.com/Chaitany840)
