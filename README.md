# 2026 FIFA World Cup · Live Tracker

A single-page live tracker for the 2026 FIFA World Cup, built with the [Taste Skill](https://github.com/Leonxlnx/taste-skill) anti-slop frontend framework.

## Features

- **Live countdown** to the next match in Indian Standard Time (IST)
- **IST clock** in the navigation bar
- **Round of 16 bracket** with tab filters (All / Results / Today / Upcoming)
- **Expandable match detail** — click a completed tie to see its Round of 32 path
- **Click-to-predict** — pick winners for upcoming matches, saved to localStorage
- **Bracket hover** — hover any match to highlight its connections to the next round
- **Golden Boot race** with goal-count visualisation
- **Road to the Final** timeline with all remaining fixtures

## Data

All match data, scores, standings, and schedules sourced from `2026-world-cup.md`. Last updated July 5, 2026.

## Open

Open `index.html` in any browser — no build step, no dependencies.

For local development, serve with Python:

```bash
python3 -m http.server 8080
# → http://localhost:8080
```

## License

MIT
