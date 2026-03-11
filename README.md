# 🛡️ SafeStreet AI v5
## World-Class AI Road Safety Platform

> *"Helping every driver, city, and responder make smarter, safer decisions — powered by Claude AI."*

**Team:** DeepBuild Warriors — AI Innovation & Competition Team
**Event:** WWV Hackathon 2026 · Challenge: Public Safety & Emergency Response
**Location:** Montgomery, Alabama

[![Deploy to Netlify](https://www.netlify.com/img/deploy/button.svg)](https://app.netlify.com/drop)

---

## 🚀 Live Demo
- **Netlify:** `https://safestreet-ai.netlify.app`
- **GitHub:** `https://github.com/YOUR_USERNAME/safestreet-ai`

---

## ✨ What's New in v5

| Feature | Description |
|---------|-------------|
| 🎯 **3-Slide Onboarding** | Beautiful intro slides with trust badges & key stats |
| 🗺️ **Community Hazard Reports** | Tap to pin potholes, debris, flooding on live map |
| 🔀 **3-Route Comparison** | Compare safe vs. fast routes with visual risk scoring |
| 🏆 **Driver Safety Score** | Gamified score based on route risk choices (0–100) |
| 🎮 **Leaderboard** | Community points for safe driving & emergency response |
| 🔊 **AI Voice Alerts** | Browser speech synthesis warns of high-risk zones |
| 📊 **Enhanced Dashboard** | 4-tab city analytics: Overview, Traffic, Leaderboard, CO₂ |
| 💫 **Premium UI** | Apple Maps–level design with smooth animations & micro-interactions |

---

## 🏗️ Full Feature Set

### 🗺️ Map Experience
- Dark Leaflet map (Carto Dark tiles) centered on Montgomery, AL
- Crash heatmap with 600+ data points from real corridors
- Hospital markers (🏥 red) + Police markers (👮 blue) with full popups
- Traffic zone labels (colored by congestion level)
- Community hazard pins — report potholes, debris, accidents, flooding
- Layer toggle panel: Heat / Hospitals / Police / Traffic / Community
- Heavy traffic banner linking to smart detour
- Hold-3s SOS activation button

### 🚦 Traffic + CO₂ Engine
- Live congestion grid (6 routes) with Heavy/Moderate/Light status
- Vehicle type selector: Sedan / SUV / Truck / Hybrid / EV / Motorcycle
- Claude AI route analysis → risk score + delays + smart detour
- 3-tab results: Analysis | Route Comparison | CO₂ Details
- CO₂ breakdown: grams / kg / trees to offset
- Annual impact projection + EV switch savings
- Vehicle CO₂ bar chart comparison

### 🔀 Safe Route Comparison
- 3 routes compared: Current / Alt B / Alt C
- Visual risk bars + traffic level + time + distance
- "Navigate via X" action button

### 🧭 Route Safety
- Origin/destination inputs with quick-fill presets
- Claude AI 0–100 risk score with animated SVG gauge
- Driver Safety Score impact panel
- Risk factors + AI recommendation + alternative route

### 🆘 SOS Emergency
- 3-second hold-to-activate with countdown
- 6-step animated workflow
- Geolocation + Nominatim reverse geocoding
- Responder dispatch + hospital identification

### 🦺 Citizen Responder Network
- Registration (Medical / EMT / First Aid)
- Ripple animation standby mode
- Incident alert → accepting/declining
- Step-by-step first responder guide
- Gamified: +50 points on completion

### ❤️ Driver Health Monitor
- Simulated rPPG BPM with 3-phase escalation
- Live ECG sparkline chart (24 bars)
- Auto emergency trigger on cardiac alert

### 📊 City Analytics Dashboard
- **Overview:** KPI cards, weekly crash chart, hotspot rankings, hazards
- **Traffic:** Live route status + 24-hour incident rate chart
- **Leaderboard:** Driver Safety Scores + Community Responder Points
- **CO₂:** Fleet breakdown + city emissions stats

### 🚗 Post-Accident Hub
- 8-step checklist with progress tracking
- Claude AI chat assistant (Alabama-specific legal context)
- Hospital finder with navigate + call
- Police stations with Alabama SR-13 form notice

### 🏆 Gamification
- Driver Safety Score (0–100) based on route choices
- Community Responder Points (+50 per response)
- City leaderboard showing top contributors
- Earn points for: safe routes, hazard reports, emergency responses

---

## 🏗️ Architecture

```
safestreet-ai/
├── index.html              ← ENTIRE APP (~448 KB, self-contained)
├── netlify.toml            ← Netlify build, headers, CSP
├── _redirects              ← SPA fallback routing
├── README.md               ← This file
├── SafeStreet_AI_v5_MasterPrompt.md  ← Rebuild prompt
└── .gitignore
```

**Tech Stack:**
- React 18 (CDN/UMD + Babel Standalone)
- Leaflet 1.9.4 + leaflet.heat
- Anthropic Claude API (`claude-sonnet-4-20250514`)
- Web Speech API (AI voice alerts)
- Browser Geolocation API + Nominatim geocoding
- Fonts: Inter + Syne + DM Mono
- Pure CSS custom properties — no framework
- All logos base64 embedded

---

## 🚀 Deploy to Netlify

### Option A: Drag & Drop (30 seconds)
1. Go to **[app.netlify.com/drop](https://app.netlify.com/drop)**
2. Drag the entire `safestreet-ai/` folder
3. 🎉 Live immediately!

### Option B: GitHub → Netlify Auto-Deploy
```bash
cd safestreet-ai
git init
git add .
git commit -m "feat: SafeStreet AI v5 — WWV Hackathon 2026"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/safestreet-ai.git
git push -u origin main
```

**Netlify settings:**
| Setting | Value |
|---------|-------|
| Build command | *(empty)* |
| Publish directory | `.` |
| Branch | `main` |

---

## 🎯 5-Minute Judge Demo

| Step | Screen | Talking Points |
|------|--------|----------------|
| 1 | Splash + Onboard | AI-powered safety intro, trust signals |
| 2 | Map | Live crash heatmap, pin a community hazard |
| 3 | Traffic | Select SUV → analyze route → show CO₂ + 3-route comparison |
| 4 | SOS | Hold 3s → 6-step emergency dispatch |
| 5 | Health | Start monitor → watch cardiac alert trigger |
| 6 | City | 4-tab dashboard: stats, traffic, leaderboard, CO₂ |
| 7 | Route | AI score + Driver Safety Score impact |

---

## 🏆 Team

**DeepBuild Warriors — AI Innovation & Competition**

Powered by **Anthropic Claude** · **Leaflet Maps** · **Montgomery Open Data**

*WWV Hackathon 2026 · Challenge Area 4: Public Safety & Emergency Response*
