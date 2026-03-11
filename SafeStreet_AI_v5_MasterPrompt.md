# SafeStreet AI v5 — Master Rebuild Prompt

> **Purpose:** This document enables any AI model to rebuild SafeStreet AI v5 from scratch.
> Copy Section 12 (the prompt block) verbatim into any AI assistant.

---

## 1. PRODUCT VISION

SafeStreet AI is a single-file HTML civic safety platform for Montgomery, Alabama. It demonstrates:
- Real AI (Claude API) for route risk scoring and traffic analysis
- Live map with crash heatmap and hazard overlays
- Emergency response coordination
- CO₂ emissions tracking per vehicle type
- Gamified driver safety scoring

**Hackathon context:** WWV Hackathon 2026 · Challenge: Public Safety & Emergency Response
**Team:** DeepBuild Warriors — AI Innovation & Competition

---

## 2. TECH STACK

| Layer | Technology |
|-------|-----------|
| Framework | React 18 via CDN UMD + Babel Standalone (zero build step) |
| Maps | Leaflet 1.9.4 + leaflet.heat (CDN) |
| AI | Anthropic Claude API — `claude-sonnet-4-20250514` |
| Voice | Web Speech API (SpeechSynthesisUtterance) |
| Fonts | Inter + Syne + DM Mono via Google Fonts CDN |
| Tiles | Carto Dark Matter (`dark_all`) |
| Output | Single `index.html` — all logos base64 embedded |

**CRITICAL:** Output must be ONE HTML file. No separate CSS/JS files. No build step. No npm.

---

## 3. DESIGN SYSTEM

### Color Palette (CSS Variables)
```css
--bg0:#040d1a  /* Darkest — app background */
--bg1:#061020  --bg2:#091628  --bg3:#0d1e35  --bg4:#122540  --bg5:#182e4e
--bd0:#162238  --bd1:#1d2f4a  --bd2:#243c5e  --bd3:#2d4d76
--tx0:#f0f7ff  /* Primary text */  --tx1:#8ab4d4  --tx2:#4a6d8a  --tx3:#253d52
--red:#e8404a  --red2:#ff6169   /* Critical / SOS */
--grn:#22d47a  --grn2:#16a85c  /* Safe / Success */
--amb:#f5a520  --amb2:#ffbe47  /* Warning / Traffic */
--blu:#3d94ff  --blu2:#66b0ff  /* Primary actions */
--pur:#9d6ef5                  /* Gamification */
--teal:#15c4b0                 /* Accent / nav gradient */
```

### Typography
- **Body/UI:** Inter (300–900)
- **Display/Titles:** Syne (700–900)
- **Numbers/BPM/Scores:** DM Mono (400, 500)

### Component Classes
```
.card    → bg2 + bd0 + r-lg + 18px padding
.csm     → card-small (r-md + 13px padding)
.glass   → rgba(6,16,32,.92) + blur(24px) + bd0 border
.card-grn / .card-blu / .card-red / .card-amb  → colored variants
.btn     → full-width button base
.btn-red / .btn-grn / .btn-blu / .btn-ghost / .btn-sm  → variants
.inp     → dark input with blue focus glow
.chip    → pill tag, bg3, clickable (add .active for blue)
.badge   → inline pill (bdg-red / bdg-grn / bdg-amb / bdg-blu / bdg-pur / bdg-teal)
.tabs / .tab.on  → tab bar component
.pbar / .pfill  → progress bar with spring animation
.co2w    → fixed CO₂ widget (bottom-right above nav)
.voice-alert  → slide-up voice alert banner
.lb-row  → leaderboard row (.me variant = blue highlight)
.tbanner → amber gradient traffic status card
.ndot    → red notification dot on nav tab
```

### Animations
```
fadeUp / fadeIn / scaleIn / slideRight / slideUp
blink / spin / pulse / ripple / float / co2up / glow / pulseDot
```

---

## 4. SCREEN ARCHITECTURE

**App flow:** Splash → 3-slide Onboarding → Map (home)

**Nav tabs (7):** MAP 🗺️ | TRAFFIC 🚦 (red dot) | ROUTE 🧭 | SOS 🆘 | HEALTH ❤️ | CITY 📊 | HELP 🚗

**Extra screens (not in nav):** NearbyScreen | ResponderScreen | Splash | Onboarding

---

## 5. SCREENS — DETAILED SPECS

### Onboarding (3 slides)
- Slide 1 (blue): "Your Road Safety AI" — trust badges, app overview
- Slide 2 (amber): "Smart Traffic Avoidance" — 3 stats (6 routes, 22 min, 100% free)
- Slide 3 (green): "CO₂ Live Tracker" — vehicle emissions comparison table
- Animated progress dots. Skip button. Final CTA: "Start Driving Safe →"

### MapHome
- Dark Leaflet map, Carto tiles, centered at [32.3792, -86.3077]
- Heatmap: leaflet.heat, 600+ pts from 10 CORRIDORS, gradient: green→amber→red→dark-red
- Hospital markers: red circle, 🏥, popup with name/addr/phone/trauma/navigate
- Police markers: blue circle, 👮, popup with name/addr/phone/navigate
- Traffic zone labels (hidden by default): colored pill boxes per TRAFFIC_ZONES
- Community hazard layer: tap to report (pothole/debris/accident/flooding), pins added to map
- Alert cards (top): dismissable glass cards with left border colored by severity
- Heavy traffic banner (amber): links to TrafficScreen
- Layer toggle panel (⚡ button): Heat/Hospitals/Police/Traffic/Community
- SOSBtn: hold 3s to trigger (fixed top-right)
- Nearby (📍) + Center (🎯) control buttons
- Community report toolbar (bottom-left): type selector, crosshair mode
- Crash density legend + "● LIVE" badge on logo

### TrafficScreen — 3 output tabs
**Inputs:** Vehicle selector (6 types with g/mi shown), origin + destination + chips, Analyze button
**Live Traffic Grid (always shown):** 2-column grid, 6 zones, badge + delay
**Tab 1 — Analysis:** Risk dial + traffic delay side-by-side | Risk factors | AI recommendation with detour
**Tab 2 — Routes:** SafeRouteComparison (3 routes with visual risk bars)
**Tab 3 — CO₂:** Trip breakdown (grams/kg/trees) | Awareness projection | Vehicle comparison bars
**Voice alert:** triggered if score ≥ 50

### SafeRouteComparison (standalone component)
- 3 routes: Route A (red, risk 74), Alt B (amber, risk 42), Alt C (green, risk 18)
- Tap to select; shows gradient risk bar, 4 stats (time/distance/traffic/risk)
- "Navigate via X" CTA when route selected

### RouteCheck
- Origin/dest inputs + quick-fill pairs
- "Start Safe Route Analysis" intro card (shown before first analysis)
- RiskDial component (shared SVG gauge)
- Driver Safety Score impact card (purple)
- Risk factors list
- AI recommendation card with alternative

### SOSScreen
- Hold-to-activate SOSBtn (3s countdown with glow animation)
- Geolocation + Nominatim reverse geocoding
- 6-step animated workflow (icons → spinning → checkmarks)
- Responder cards appear at step 3

### ResponderScreen — state machine
States: list → reg → standby (3s auto→alert) → alert → responding → done
- List: Stats grid (active/nearest/ETA) + leaderboard preview + active responders
- Reg: 3 training level cards with points badges
- Standby: ripple animation
- Alert: incident details card + respond/decline buttons
- Responding: navigate CTA + numbered first-responder guide
- Done: +50 points celebration + badges

### HealthMonitor
- BPM display (large DM Mono, color-coded, pulse animation speed = bpm)
- Sequence: Normal 70-74 → Elevated 94-112 → Cardiac 144+ → 0 triggers alert
- ECG chart: 24 bars, height proportional to BPM
- Phase indicator card
- Cardiac Alert screen: auto-dispatch panel (911/SMS/Hospital/Police)
- Voice alert on cardiac detection

### Dashboard — 4 tabs
**Overview:** KPI cards (Crashes/Response/Alerts/Responders) | Weekly bar chart | Hotspot rankings | Hazards
**Traffic:** Live route status with pulsing dots | 24-hour incident rate bar chart (24 bars)
**Leaderboard:** User safety score card | LEADERBOARD array + points guide
**CO₂:** City emissions stats (3 KPIs) | Fleet breakdown bars | EV impact message

### PostAccident — 4 tabs
**Steps:** 8-item checklist with progress bar + spring animation
**AI Help:** Full conversation UI, 3 quick prompts, Claude API powered, Alabama SR-13 context
**Hospitals:** Cards with navigate + call
**Police:** Alabama law notice + 4 precincts

### NearbyScreen
- Call 911 + MPD Direct buttons (red/blue)
- Hospital/Police tabs
- Navigate + Call per item

---

## 6. SHARED COMPONENTS

### RiskDial({score, size=130})
SVG circular gauge, 0-100 range, color = riskColor(score), inner bg circle, score text + level text

### SOSBtn({onTrigger})
Fixed top-right, 52x52, hold 3s countdown, red glow animation, triggers callback

### CO2Widget({miles, vType, onOpen})
Fixed bottom-right above nav, appears at ≥0.04 miles, shows grams, animated CO₂ particles rising

### VoiceAlertBanner({msg, onDismiss})
Slide-up banner, calls speechSynthesis, auto-dismisses after 6s, also dismissable by tap

---

## 7. DATA CONSTANTS

### TRAFFIC_ZONES (6)
```js
[
  {name:"Atlanta Hwy (US-80)",level:"heavy",delay:18,color:"#ef4444",lat:32.385,lng:-86.262},
  {name:"I-85 Northbound",level:"moderate",delay:8,color:"#f59e0b",lat:32.382,lng:-86.288},
  {name:"Eastern Blvd",level:"heavy",delay:22,color:"#ef4444",lat:32.371,lng:-86.244},
  {name:"Ann Street",level:"light",delay:2,color:"#22c55e",lat:32.378,lng:-86.301},
  {name:"Troy Hwy (US-231)",level:"moderate",delay:11,color:"#f59e0b",lat:32.325,lng:-86.278},
  {name:"Narrow Lane Rd",level:"light",delay:3,color:"#22c55e",lat:32.339,lng:-86.335},
]
```

### CO2_FACTORS (g/mile)
```js
{sedan:251, suv:354, truck:414, hybrid:133, electric:0, motorcycle:107}
```

### SAFE_ROUTES (3-route comparison)
```js
[
  {id:"A",name:"Current Route",color:"#ef4444",risk:74,time:22,dist:"8.4 mi",traffic:"Heavy"},
  {id:"B",name:"Alt Route B",color:"#f59e0b",risk:42,time:19,dist:"9.1 mi",traffic:"Moderate"},
  {id:"C",name:"Scenic Alt C",color:"#22c55e",risk:18,time:26,dist:"11.2 mi",traffic:"Light"},
]
```

### LEADERBOARD (5 entries)
```js
[
  {rank:1,name:"Dr. Keisha Williams",pts:1240,badge:"🏆",role:"RN"},
  {rank:2,name:"Marcus Johnson",pts:980,badge:"🥈",role:"EMT"},
  {rank:3,name:"Sarah Chen",pts:720,badge:"🥉",role:"FA"},
  {rank:4,name:"James Rivera",pts:610,badge:"⭐",role:"Driver"},
  {rank:5,name:"You",pts:125,badge:"🔰",role:"Driver",isMe:true},
]
```

### COMMUNITY_HAZARD_TYPES
```js
[
  {id:"pothole",label:"Pothole",icon:"🕳️",color:"#f59e0b"},
  {id:"debris",label:"Debris",icon:"🪨",color:"#94a3b8"},
  {id:"accident",label:"Accident",icon:"💥",color:"#ef4444"},
  {id:"flooding",label:"Flooding",icon:"🌊",color:"#3b82f6"},
]
```

### WEEKLY (crash counts by day)
```js
[32, 45, 28, 61, 54, 38, 43]  // Mon-Sun (Friday highest)
```

---

## 8. CLAUDE AI INTEGRATION

### Helper function
```js
async function askClaude(prompt, sys, max=900) {
  const r = await fetch('https://api.anthropic.com/v1/messages', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      model: 'claude-sonnet-4-20250514',
      max_tokens: max,
      system: sys,
      messages: [{ role: 'user', content: prompt }]
    })
  });
  const d = await r.json();
  return d.content?.[0]?.text || null;
}
```

### Traffic + Route Analysis prompt
```
Route "${origin}" to "${dest}" Montgomery AL ~${dist}mi.
HEAVY: ${heavy}. MODERATE: ${mod}.
Vehicle: ${vType}, CO2: ${co2.grams}g.
Return ONLY valid JSON:
{"score":number,"level":"LOW|MODERATE|HIGH|CRITICAL","exp":"string",
"risks":["r1","r2","r3"],"rec":"string","alt":"string",
"trafficDelay":number,"altDelay":number,"co2msg":"string"}
```
System: `You are a traffic + route risk AI for Montgomery AL. Reply ONLY with valid compact JSON.`

### Post-Accident chat
System: `You are SafeStreet AI post-accident assistant for Montgomery AL. Be calm, 2-3 sentences, practical. Alabama: SR-13 form required within 30 days for $500+ damage or any injury.`

### Fallback
All AI calls: `try { JSON.parse(raw.replace(/```json|```/g,'').trim()) } catch { use mock }`
Never show errors to user. Always have a result.

---

## 9. VOICE ALERTS

```js
function speak(text) {
  if (!window.speechSynthesis) return;
  const u = new SpeechSynthesisUtterance(text);
  u.rate = 0.95; u.pitch = 1;
  speechSynthesis.cancel();
  speechSynthesis.speak(u);
}
```

Triggered when:
- Route risk score ≥ 50 (after Route Check or Traffic analysis)
- Cardiac alert detected (Health Monitor)
- Community hazard submitted

---

## 10. LOGOS

Three base64 constants (encode at build time):
```js
const IMG_ICON = "data:image/jpeg;base64,[ICON_B64]";   // Shield icon, used in Splash + favicon
const IMG_MAIN = "data:image/jpeg;base64,[MAIN_B64]";   // Full logo+text, used in headers
const IMG_TEAM = "data:image/png;base64,[TEAM_B64]";    // DeepBuild Warriors, used in footer
```

Encode: `base64 logo.png | tr -d '\n' > logo.b64`

---

## 11. DEPLOYMENT FILES

### netlify.toml
```toml
[build]
  publish = "."
  command = ""
[[headers]]
  for = "/*"
  [headers.values]
    Permissions-Policy = "camera=*, geolocation=*, microphone=*"
    Content-Security-Policy = "default-src 'self' 'unsafe-inline' 'unsafe-eval' https://unpkg.com https://fonts.googleapis.com https://fonts.gstatic.com https://*.cartocdn.com https://api.anthropic.com https://nominatim.openstreetmap.org data:; img-src * data: blob:; connect-src *; media-src *;"
[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200
```

### _redirects
```
/*    /index.html   200
```

---

## 12. MASTER REBUILD PROMPT

> Copy everything below this line and paste into any AI model to rebuild the app.

---

```
You are a world-class frontend engineer. Build SafeStreet AI v5 — a complete, production-ready
civic safety app as ONE self-contained HTML file (index.html). No build step, no npm, no external files.

TECH: React 18 CDN + Babel Standalone + Leaflet 1.9.4 + leaflet.heat. Claude API for AI features.
LOCATION: Montgomery, Alabama

ALL LOGOS must be provided as base64 data URIs embedded in JS constants:
  const IMG_ICON = "data:image/jpeg;base64,..." (shield icon, use for favicon + splash)
  const IMG_MAIN = "data:image/jpeg;base64,..." (full wordmark logo)
  const IMG_TEAM = "data:image/png;base64,..."  (DeepBuild Warriors team logo)

DESIGN SYSTEM: Dark theme with CSS custom properties.
Background scale: #040d1a → #061020 → #091628 → #0d1e35 → #122540
Colors: --red:#e8404a --grn:#22d47a --amb:#f5a520 --blu:#3d94ff --pur:#9d6ef5 --teal:#15c4b0
Text: --tx0:#f0f7ff --tx1:#8ab4d4 --tx2:#4a6d8a
Fonts: Inter (body), Syne (display/titles), DM Mono (numbers) via Google Fonts CDN

SCREENS (bottom nav 7 tabs + extras):

1. SPLASH: Animated icon (float+ripple), main logo, progress bar (100% in ~7s), trust badges, team logo footer

2. ONBOARDING (3 slides): Progress dots. Slide 1 (blue) = overview + trust badges. Slide 2 (amber) = traffic stats. Slide 3 (green) = CO₂ table. Skip button. Final CTA = "Start Driving Safe →"

3. MAP: Leaflet dark map, center=[32.3792,-86.3077], crash heatmap (leaflet.heat), hospital markers (red 🏥), police markers (blue 👮), traffic zone labels (hidden by default), community hazard pins, layer toggle panel, alert cards top (dismissable), heavy traffic banner, SOS hold-button (top-right), community hazard report mode (tap to pin pothole/debris/accident/flooding)

4. TRAFFIC+CO2: Live traffic grid (6 zones, 2-col), vehicle selector (6 types showing g/mi), route inputs + chips, Claude AI analyze button, 3-tab output: Analysis (RiskDial + delays) | Routes (3-route comparison cards with risk bars) | CO₂ (grams/kg/trees + awareness + vehicle chart). Voice alert if score≥50.

5. ROUTE: Simple route inputs, Claude AI risk score 0-100 (RiskDial SVG component), Driver Safety Score impact card (purple), risk factors list, AI recommendation card

6. SOS: Hold-3s SOSBtn trigger, 6-step workflow (GPS→911→Family→Responders→Hospital→Police), each step shows pending/active(spin)/done(check), responder cards appear at step 3

7. HEALTH: BPM large display, 3-phase simulation (Normal→Elevated→Cardiac), 24-bar ECG chart, phase indicator card, cardiac alert triggers emergency screen + voice alert

8. CITY DASHBOARD (4 tabs): Overview (KPIs+chart+hotspots+hazards) | Traffic (route status+24h chart) | Leaderboard (safety score + points + earn guide) | CO₂ (city emissions + fleet breakdown)

9. POST-ACCIDENT (4 tabs): Checklist (8 items, progress) | AI Chat (Claude) | Hospitals | Police (Alabama law card)

10. NEARBY: 911/MPD emergency buttons, hospital+police tabs with navigate+call

11. RESPONDER: list→reg→standby(3s→alert)→responding→done(+50pts), leaderboard preview, gamification

SHARED COMPONENTS:
- RiskDial({score,size}): SVG circular gauge, colors by risk level
- SOSBtn({onTrigger}): fixed top-right, hold 3s, glow animation
- CO2Widget({miles,vType,onOpen}): fixed bottom-right above nav, shows grams, animated particles
- VoiceAlertBanner({msg,onDismiss}): slide-up, calls speechSynthesis, auto-dismiss 6s

DATA CONSTANTS (Montgomery AL):
- CENTER = [32.3792, -86.3077]
- TRAFFIC_ZONES: 6 zones (Atlanta Hwy heavy+18m, I-85 moderate+8m, Eastern Blvd heavy+22m, Ann St light, Troy Hwy moderate+11m, Narrow Lane light)
- CO2_FACTORS: {sedan:251,suv:354,truck:414,hybrid:133,electric:0,motorcycle:107}
- SAFE_ROUTES: 3 routes (A=risk74, B=risk42, C=risk18)
- LEADERBOARD: 5 entries with pts and isMe flag
- COMMUNITY_HAZARD_TYPES: pothole/debris/accident/flooding
- HOSPITALS: 4 hospitals with coords, phones, trauma flags, dist, eta
- POLICE: 4 precincts with coords, phones, dist, eta
- CORRIDORS: 10 crash intersections with risk scores
- WEEKLY: [32,45,28,61,54,38,43]

CLAUDE API:
- Model: claude-sonnet-4-20250514
- Traffic prompt returns JSON: {score,level,exp,risks[],rec,alt,trafficDelay,altDelay,co2msg}
- Route prompt returns JSON: {score,level,exp,risks[],rec,alt}
- Post-accident: conversational, Alabama SR-13 law context
- All API calls: always have fallback mock data, never show errors

OUTPUT: One index.html file, all 18 components, all screens working, logos embedded, mobile-first.
```

---

*SafeStreet AI v5 · DeepBuild Warriors · WWV Hackathon 2026 · Powered by Anthropic Claude*
