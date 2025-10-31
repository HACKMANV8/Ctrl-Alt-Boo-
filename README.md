# Ctrl-Alt-Boo-
🚀 Upskill Navigator

Your all-in-one personalized learning assistant — built to guide, motivate, and upskill users efficiently through smart AI tools and gamification.

## 🧠 Overview

Upskill Navigator helps learners plan, track, and enhance their skill development journey with AI-powered guidance, real-time streaks/timers, and a synced leaderboard among participants.

## 🧩 Features

- *Auth*: Firebase Authentication (email/password)
- *AI Chatbot*: Gemini (2.5 Flash) via a dev-only Vite proxy at POST /api/chat
- *Leaderboard*: Ranks participants (users.isParticipant==true) by total points; weekly points from recent quiz scores
- *Quizzes*: Writes scores and adds user points; marks users as participants
- *Streaks*: Current and best streak computed from logins/{uid}/days
- *Screen Time*: Live per-day timer shared across Home and Streak (screenTime/{uid}/days/{YYYY-MM-DD})
- *Doubt Solver*: Create doubts, AI-generated answers, status and resolved filtering

## ⚙ Tech Stack

- Frontend: React + Tailwind CSS + Vite
- Backend: Firebase (Auth, Firestore)
- AI: Google Gemini (2.5 Flash)
- Hosting: Vercel/Firebase Hosting (dev proxy is Vite-only)
- Database: Firestore

## 🔧 Environment

Create a .env in project root:


GOOGLE_API_KEY=YOUR_GEMINI_KEY


Notes:
- The Vite dev server reads .env on startup. Restart after changes.
- The /api/chat proxy is available only in npm run dev. For production, add a serverless function (Netlify/Vercel/Firebase) and point the client to it.

## ▶ Development

bash
npm install
npm run dev


- Visit the Chatbot page and ask something to test Gemini.
- Take a Quiz to generate scores and points; you’ll appear on the leaderboard.
- Use Streak page to “Mark Today” and see streak/best-streak on Home.
- Start/Stop the Screen Time timer on Home or Streak and see values sync.
- Submit a doubt in Doubt Solver; an AI answer will be added automatically.

## 🗂 Firestore Collections (per user where applicable)

- users/{uid}: { totalPoints: number, isParticipant: boolean, displayName?, photoURL? }
- scores/{autoId}: { uid, score, subject, createdAt } (weekly points derive from last 7 days)
- logins/{uid}/days/{YYYY-MM-DD}: { active: true, at }
- streaks/{uid}: { streak, lastActive, updatedAt }
- screenTime/{uid}/days/{YYYY-MM-DD}: { totalMs, running, lastStart, updatedAt }
- doubts/{uid}/items/{autoId}: { question, answer, status: 'pending'|'answered'|'error', resolved, createdAt, updatedAt }

Ensure your Firestore rules allow authenticated users to read/write their own documents under these paths.

## 🚀 Production note

- Replace the Vite dev proxy with a serverless function for /api/chat and configure GOOGLE_API_KEY as a server-side secret.
