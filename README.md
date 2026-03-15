# 🔥 SRH Fan App — Orange Army HQ

> A full-stack fan experience app for Sunrisers Hyderabad built for IPL 2026.  
> Designed and developed end-to-end as a personal project to demonstrate product thinking, API strategy, and rapid prototyping skills.

![Tech Stack](https://img.shields.io/badge/Frontend-React_+_Vite_+_Tailwind-orange)
![Backend](https://img.shields.io/badge/Backend-FastAPI_+_Python-blue)
![APIs](https://img.shields.io/badge/APIs-CricketData.org_%7C_Open--Meteo-green)
![Status](https://img.shields.io/badge/Status-Active-brightgreen)

---

## 📌 Product Overview

Most cricket fan apps are generic scoreboards. This project asks a different question: **what does a fan actually want during a match?**

The answer is more than just a score — it's knowing your squad inside out, tracking the fixture calendar, checking if rain will ruin the game, and seeing live ball-by-ball action. This app brings all of that into one focused, SRH-specific experience.

### Core Features

| Feature | Description |
|---|---|
| 🏏 **Squad Explorer** | Full IPL 2026 squad (25 players) with T20 career stats, role filters, player bios, and real player headshots |
| 📅 **Match Schedule** | All SRH fixtures with live countdown timers, home/away indicators, and match-day weather per venue |
| ⚡ **Live Match Centre** | Live scorecard with batting & bowling tables, auto-refreshing every 30 seconds |
| 🌤️ **Weather Intelligence** | Real-time match-day weather and rain probability for each venue — so you know before the toss |

---

## 🧠 Product Thinking

### Problem
IPL fans follow one team passionately but existing apps serve the entire tournament. Finding SRH-specific information — squad depth, next fixture, live score — requires navigating cluttered, ad-heavy platforms.

### Solution
A zero-clutter, single-team app that surfaces exactly what an SRH fan needs, exactly when they need it.

### API Strategy — 100% Free Tier
A key constraint I set for this project was **zero API cost**. This forced deliberate choices:

| Decision | Reasoning |
|---|---|
| CricketData.org (100 hits/day) | Sufficient for personal use with aggressive caching and 30s polling |
| Open-Meteo (no key, no limits) | Best free weather API — no rate limits, no signup required |
| Cricinfo image proxy | Cricinfo blocks hotlinking; proxying server-side bypasses this cleanly |
| Seed data for squad/schedule | Static data avoids burning API quota on non-live information |

### Rate Limit Management
The live match page polls every **30 seconds** deliberately — not every 5. At 30s intervals during a 3.5-hour match, that's ~420 hits. Combined with other endpoints, this stays comfortably within the 100 hits/day free tier for personal use.

---

## 🛠️ Technical Architecture

```
srh-app/
├── backend/                    # FastAPI (Python)
│   └── app/
│       ├── main.py             # App entry point + CORS
│       ├── data/
│       │   ├── squad.py        # SRH 2026 squad seed data (25 players)
│       │   └── schedule_data.py # IPL 2026 fixture data
│       └── routers/
│           ├── squad.py        # GET /api/squad/
│           ├── schedule.py     # GET /api/schedule/
│           ├── matches.py      # GET /api/matches/live  ← proxies CricketData.org
│           ├── weather.py      # GET /api/weather/{city} ← Open-Meteo
│           └── images.py       # GET /api/images/{id}  ← Cricinfo image proxy
└── frontend/                   # React + Vite + Tailwind
    └── src/
        ├── App.jsx             # Router + Navbar
        ├── pages/
        │   ├── SquadPage.jsx   # Squad list + player modal
        │   ├── SchedulePage.jsx # Fixture cards + weather
        │   └── MatchPage.jsx   # Live scorecard
        ├── components/
        │   ├── PlayerCard.jsx  # Full stats modal
        │   ├── WeatherWidget.jsx
        │   └── Skeleton.jsx    # Loading states
        └── hooks/
            └── useApi.js       # Fetch + polling hooks
```

**Key design decisions:**
- Vite's dev proxy (`/api → localhost:8000`) means zero CORS config needed during development
- Image proxy router on the backend bypasses Cricinfo's hotlink protection without storing any images locally
- Mock live match data ships by default — the app is fully functional without any API key

---

## 🚀 Running Locally

**Prerequisites:** Python 3.13, Node.js

```bash
# 1. Install backend dependencies
cd backend
pip install fastapi uvicorn[standard] httpx python-dotenv

# 2. Start backend
python -m uvicorn app.main:app --reload --port 8000

# 3. In a new terminal, start frontend
cd frontend
npm install
npm run dev
```

App runs at **http://localhost:5173** · API docs at **http://localhost:8000/docs**

**Optional:** Add a free CricketData.org API key to `backend/.env` to enable real live scores. Without it, the app uses realistic mock match data.

---

## 📈 What's Next

Features planned for future iterations:

- **GNews integration** — SRH-specific news feed using the free GNews API
- **Fan polling** — Match prediction polls with local result tracking
- **AI match analysis** — Post-match summary using Claude API
- **Points table** — IPL standings with SRH highlighted
- **Push notifications** — Browser alerts on SRH wickets and milestones

---

## 👤 About

Built by **Hriday** — a product manager who believes the best way to sharpen product instincts is to build things yourself.

This project demonstrates:
- Translating a user need into a scoped, shippable product
- Making deliberate technical trade-offs under constraints (zero API cost)
- End-to-end ownership from architecture to UI

---

*🧡 Go SRH. Rise Together.*# SRH-fan-app
An app for the fans of Sunrisers Hyderabad
