# Desktop Multimedia Recommender

> A desktop application that takes natural language prompts and returns ranked, explainable media recommendations across movies, series, music, and games.

<!-- TODO: Add a screenshot or short demo GIF here -->
<!-- Example: ![App Screenshot](docs/screenshot.png) -->

---

## What It Does

Type anything in plain language — the app understands your intent, fetches live metadata, scores candidates, and returns explainable recommendation cards.

**Example prompts:**
- `90s crime series with a Twin Peaks-like atmosphere`
- `Italian mafia films`
- `uplifting Korean drama from the last 5 years`
- `dark sci-fi like Black Mirror`

The recommendation engine works **without an LLM** — AI is optional and enhances intent parsing, but the core pipeline runs fully offline with a built-in sample catalog.

---

## Features

- **Natural language input** — Turkish and English supported
- **Live API integration** — TMDB for movies & series, Spotify for music
- **Explainable results** — every card shows why it was recommended
- **Weighted scoring** — semantic similarity 35%, genre 20%, type 15%, mood 10%, era 10%, rating 5%, popularity 5%
- **Modular AI providers** — Offline Basic / OpenAI-compatible (Groq) / LM Studio
- **Graceful fallback** — works with zero API keys using a built-in sample catalog
- **Local caching** — SQLite database stores fetched metadata

---
## Screenshot
<img width="1182" height="751" alt="User attachment" src="https://github.com/user-attachments/assets/56d950c1-487e-4372-8aa6-f2d4642f317a" />

---

## Quick Start

**Requirements:** Python 3.10+

```bash
# 1. Clone
git clone https://github.com/LordAlis/desktop-multimedia-recommender.git
cd desktop-multimedia-recommender

# 2. Create virtual environment and install dependencies
python3 -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt

# 3. Configure API keys (optional but recommended)
cp .env.example .env
# Edit .env and add your keys

# 4. Run
python main.py
```

> **No API keys?** The app still launches and recommends from a built-in sample catalog.

---

## API Keys

All keys are free. Add them to `.env` for the best experience.

| Key | Get it from | Enables |
|-----|-------------|---------|
| `TMDB_API_KEY` | [themoviedb.org/settings/api](https://www.themoviedb.org/settings/api) | Live movie & series search |
| `OPENAI_API_KEY` | [console.groq.com/keys](https://console.groq.com/keys) (Groq — free, no card) | Smart intent parsing via LLM |
| `OMDB_API_KEY` | [omdbapi.com/apikey.aspx](https://www.omdbapi.com/apikey.aspx) | IMDb rating enrichment |
| `SPOTIFY_CLIENT_ID/SECRET` | [developer.spotify.com/dashboard](https://developer.spotify.com/dashboard) | Music recommendations |

---

## Architecture

```
ui/                    PyQt6 — main window, chatbot panel, recommendation cards
core/chatbot/          Provider-based chatbot (Offline / OpenAI-compatible / LM Studio)
core/recommendation/   Intent parser, scoring engine, explanation builder
integrations/          API clients: TMDB, OMDb, Spotify
data/                  SQLite repositories, domain models
app/                   Config, constants, logging
```

**Recommendation pipeline:**

```
Natural language prompt
        ↓
  Intent Parser  ──── (LLM if available, keyword fallback otherwise)
        ↓
  Candidate Fetch ─── TMDB / Spotify / SQLite cache / sample catalog
        ↓
  Scoring Engine  ─── weighted multi-signal ranking
        ↓
  Explainable Cards
```

---

## AI Modes

| Mode | Description |
|------|-------------|
| **Offline Basic** | Template-based responses, no internet or API keys needed |
| **OpenAI-Compatible API** | Any OpenAI-compatible endpoint (Groq, OpenAI, vLLM, etc.) |
| **LM Studio Local** | Local LLM via [LM Studio](https://lmstudio.ai) |

---

## Running Tests

```bash
pip install pytest
python -m pytest tests/ -v
```

---

## Built With

- [Python 3.10+](https://python.org)
- [PyQt6](https://pypi.org/project/PyQt6/) — desktop GUI
- [SQLite](https://sqlite.org) — local caching
- [TMDB API](https://developer.themoviedb.org) — movie & series metadata
- [Groq](https://groq.com) — free LLM inference (optional)
