# ⚽ 2026 World Cup — Interactive Project Brainstorm

> A collection of ideas, data schemas, and implementation paths for building something fun with the World Cup data.  
> **Data source:** `2026-world-cup.md` (all matches, results, standings, times in IST)  
> **Design toolkit:** `taste-skill/` (anti-slop frontend design rules for AI agents)

---

## 📦 What Data You Have

The file `2026-world-cup.md` contains structured, parseable data covering:

| Data | Format | Count |
|------|--------|-------|
| Group standings (A–L) | Markdown tables | 12 groups × 4 teams |
| Group stage match results | Table rows with date, teams, score, IST time | 72 matches |
| Round of 32 results | Table rows | 16 matches |
| Round of 16 schedule & results | Table rows | 8 matches (2 played, 6 upcoming) |
| Quarterfinal schedule | Table rows | 4 matches |
| Semifinal schedule | Table rows | 2 matches |
| Third place & Final schedule | Table rows | 2 matches |
| Top scorers | Table rows | 8+ players |
| Prize money breakdown | Table rows | 7 tiers |
| Venue details | Table rows | 16 stadiums |
| Quick IST reference | Table rows | 14 upcoming matches |

### What you can derive from this data:

- **Countdown to next match** — parse the IST date/time of upcoming fixtures
- **Live bracket state** — track which teams advanced through each knockout round
- **Group stage narratives** — who overperformed, who crashed out, debutant stories
- **Top scorer race** — Messi vs Mbappé at 7 goals each, Kane & Haaland at 5
- **Host nation tracker** — 🇲🇽 Mexico (perfect group stage), 🇺🇸 USA (still alive), 🇨🇦 Canada (eliminated R16)
- **Attendance records** — tournament already broke all-time record with 44 matches remaining
- **Cinderella stories** — 🇨🇻 Cape Verde (drew with Spain!), 🇲🇦 Morocco (knocked out Netherlands on pens)

---

## 🎯 Project Ideas

### 1. 🖥️ Live Match Tracker — Single-Page Web App

**Concept:** A beautiful one-page dashboard showing everything at a glance.

**Sections:**
- **Hero banner** — "Now Playing" or "Next Up" with countdown timer in IST
- **Today's matches** — cards showing teams, flags, kickoff time, live score placeholder
- **Completed matches timeline** — scrollable feed with scores and key moments
- **Mini bracket** — compact view of the knockout tree
- **Stats bar** — top scorers, total goals, attendance

**Tech:** Plain HTML/CSS/JS (no framework needed), parse the `.md` into JSON once. Optionally use taste-skill's design rules for layout/motion/spacing.

**Why this first:** It's the fastest to build, gives you something visual immediately, and every new match result is a satisfying update.

---

### 2. 🏟️ Interactive Tournament Bracket

**Concept:** A full 48-team bracket visualization. Click any match to see the result, click forward to predict.

**Features:**
- Rounds: Group → R32 → R16 → QF → SF → Final
- Zoomable/pannable (104 matches is a lot of real estate)
- Color-coded by confederation (UEFA blue, CONMEBOL green, AFC red, etc.)
- Hover tooltips with match details, scorers, attendance
- **Prediction mode:** before a match is played, let the user click who they think wins. Score their bracket against reality.

**Tech:** D3.js or plain SVG/CSS Grid. The data structure is a tree — perfect for recursive rendering.

---

### 3. 🕐 IST Match Countdown Widget

**Concept:** A minimal widget showing time until the next World Cup match, in Indian time.

**Features:**
- "Next match: 🇧🇷 Brazil vs 🇳🇴 Norway — 1:30 AM IST (in 3h 22m)"
- Auto-advances to next match after kickoff
- Optional: macOS menu bar app (SwiftUI), Raycast extension, or HTML widget for Notion/Obsidian
- Click to expand full upcoming schedule

**Tech:** Vanilla JS with `setInterval`, or Python script for terminal. Dead simple, hyper useful.

---

### 4. 📊 Terminal Dashboard (CLI)

**Concept:** A `worldcup` command you run in your terminal.

**Commands:**
```bash
worldcup now            # current match or next match
worldcup bracket        # ASCII art bracket
worldcup group A        # Group A standings
worldcup scorers        # top scorers leaderboard
worldcup today          # all matches scheduled today (IST)
worldcup watch          # live-refreshing dashboard
```

**Tech:** Python (Rich library for colored tables) or Node.js (chalk, boxen). Parse `2026-world-cup.md` at startup.

---

### 5. 🗺️ Stadium Explorer Map

**Concept:** Interactive map of the 16 host stadiums across US, Mexico, Canada.

**Features:**
- Mapbox/Leaflet map with stadium markers
- Click a stadium → see all matches played there, attendance numbers, photos
- Filter by host country or match stage
- Timeline scrubber (June 11 → July 19) to see matches move across the map

**Data you already have:** Venue names, cities, countries, capacities. Each match in `2026-world-cup.md` maps to a venue.

---

### 6. 🏆 "You Be the Pundit" — Prediction Game

**Concept:** Before each knockout match, you predict the winner. Track your accuracy.

**Features:**
- Show upcoming match, two team flags, a "Pick Winner" button
- After the match, reveal the actual result and whether you were right
- Running score: "You've called 11/16 matches correctly (68.8%)"
- Shareable results card

**Storage:** `localStorage` — no backend needed.

---

### 7. 📱 iOS Widget (SwiftUI)

**Concept:** A lock-screen or home-screen widget for iPhone showing the next World Cup match time in IST.

**Features:**
- Small widget: next match + countdown
- Medium widget: today's matches + scores
- Taps open the full bracket in a companion app or Safari

**Tech:** SwiftUI `WidgetKit`. Would pair well with your iOS development experience.

---

## 🧱 Recommended Data Schema (JSON)

Extract from `2026-world-cup.md` into a structure like this for any web/CLI project:

```json
{
  "tournament": {
    "name": "2026 FIFA World Cup",
    "hosts": ["Canada", "Mexico", "United States"],
    "startDate": "2026-06-11",
    "endDate": "2026-07-19"
  },
  "groups": {
    "A": {
      "standings": [
        { "team": "Mexico", "flag": "🇲🇽", "played": 3, "won": 3, "drawn": 0, "lost": 0, "gf": 6, "ga": 0, "gd": 6, "pts": 9 },
        { "team": "South Africa", "flag": "🇿🇦", "played": 3, "won": 1, "drawn": 1, "lost": 1, "gf": 2, "ga": 3, "gd": -1, "pts": 4 }
      ],
      "matches": [
        { "date": "2026-06-11", "home": "Mexico", "away": "South Africa", "score": [2, 0], "ist": "2026-06-12T00:30:00+05:30", "venue": "Estadio Azteca", "city": "Mexico City" }
      ]
    }
  },
  "knockout": {
    "roundOf32": [
      { "id": 73, "date": "2026-06-28", "home": "South Africa", "away": "Canada", "score": [0, 1], "winner": "Canada", "ist": "2026-06-29T00:30:00+05:30" }
    ],
    "roundOf16": [],
    "quarterfinals": [],
    "semifinals": [],
    "thirdPlace": null,
    "final": null
  },
  "topScorers": [
    { "player": "Lionel Messi", "country": "Argentina", "flag": "🇦🇷", "goals": 7 },
    { "player": "Kylian Mbappé", "country": "France", "flag": "🇫🇷", "goals": 7 }
  ]
}
```

---

## 🎨 Design Direction (via taste-skill)

The cloned `taste-skill/` repo gives AI coding agents design rules. For a World Cup tracker, you'd want:

- **Vibe:** Energetic, stadium-feel, big typography. Dark background with pitch-green accents. Think broadcast sports graphics, not corporate dashboard.
- **Motion:** Score reveal animations, bracket line animations, countdown ticker. GSAP scroll-triggered reveals for the match timeline.
- **Spacing:** Generous — this is celebration content, not a data table. Big flags, big scores, breathing room.
- **Fonts:** A bold display font for scores/headlines (think: stadium scoreboard), clean sans-serif for data.

The taste-skill SKILL.md files enforce things like:
- No em-dashes in UI copy
- No generic gray backgrounds
- Proper font hierarchy
- GSAP for motion (not CSS animations)
- Dark/light intentional color palettes

---

## 🚀 Suggested Build Order

| Priority | Project | Effort | Why First |
|----------|---------|--------|-----------|
| **1** | Live Match Tracker (web app) | 3–4 hours | Most visual, most fun, uses all data |
| **2** | IST Countdown Widget | 30 min | Instant utility for you personally |
| **3** | Interactive Bracket | 2–3 hours | Natural extension of #1 |
| **4** | Terminal CLI | 1–2 hours | Quick win, useful for quick checks |
| **5** | Prediction Game | 2 hours | Adds interactivity layer to #1 |
| **6** | Stadium Map | 3–4 hours | Visual variety, geographic context |
| **7** | iOS Widget | 2–3 hours | Personal use, pairs with your iOS skills |

---

## 🔗 Key Files in This Workspace

| File | Purpose |
|------|---------|
| `2026-world-cup.md` | Complete tournament data (all times in IST) |
| `taste-skill/skills/taste-skill/SKILL.md` | Main design rules for AI frontend generation |
| `taste-skill/skills/gpt-tasteskill/SKILL.md` | Stricter GPT/Codex design rules |
| `taste-skill/skills/soft-skill/SKILL.md` | Premium calm UI direction |
| `taste-skill/skills/minimalist-skill/SKILL.md` | Editorial/product-style minimal UI |

---

> *Ready to build? Pick idea #1 and say "scaffold it" — I'll generate the full HTML/CSS/JS with taste-skill design rules baked in.*
