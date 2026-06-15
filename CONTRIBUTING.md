# Contributing to Goal Digger

Thanks for your interest in contributing! Goal Digger is an open reference app built on the Aomi SDK. This guide will get you from zero to your first PR.

---

## Prerequisites

- Rust (latest stable) — install via [rustup.rs](https://rustup.rs)
- Node.js 18+ for the UI
- Python 3.10+ for data prep (optional)
- An Aomi account — sign up at [aomi.dev](https://aomi.dev)

---

## Getting Started

1. Fork this repo and clone your fork

```bash
git clone https://github.com/YOUR_USERNAME/goal-digger.git
cd goal-digger
```

2. Build the Aomi plugin

```bash
cd app
cargo build --release
```

3. Run the dashboard

```bash
cd server
cargo run --release
```

4. Open http://127.0.0.1:8787 in your browser

> **Note:** Live match data requires a paid API-FOOTBALL key. Without it the app falls back to bundled sample data — this is fine for development and contributions.

---

## Project Structure

```
goal-digger/
  app/          Aomi plugin (Rust). The simulation engine and 6 typed tools live here.
    src/sim.rs    the engine (team strength → Dixon-Coles → 50k Monte-Carlo)
    src/tool.rs   simulate_match, simulate_tournament, find_edge, get_team_dossier,
                  get_wc_fixtures, watch_match
    src/data.rs   team strength loader + API-FOOTBALL + Polymarket Gamma price fetch
    data/teams.json   bundled Elo + xG per team
  server/       HTTP server that serves the dashboard using the same engine.
  ui/           React dashboard. Board, Match Detail, and Edges views.
  data-prep/    Python script that pulls team xG and Elo into teams.json.
```

---

## Good First Issues

Look for issues tagged [`good first issue`](https://github.com/victorchimakanu/goal-digger/issues?q=label%3A%22good+first+issue%22) on the issues page. Here are some areas to start:

- **UI improvements** — the dashboard uses a retro Camel Brown theme, there's room to improve responsiveness and loading states
- **Unit tests** — add unit tests for `sim.rs` probability outputs, the engine needs test coverage
- **Error handling** — improve error messages in server API endpoints
- **Documentation** — improve inline code comments in `tool.rs` and `sim.rs`
- **Data prep** — improve the Python script for pulling fresh team data
- **Mobile layout** — add a responsive mobile layout to the dashboard

---

## API Reference (Quick)

| Endpoint | Returns |
|---|---|
| `GET /` | the camel-theme UI |
| `GET /api/board` | all fixtures priced by the engine |
| `POST /api/simulate` | one match: `{home, away, neutral, knockout, *_adj, seed, sims}` |
| `GET /api/edge?slug=&outcome=&model_prob=` | model vs live Polymarket (Gamma) price |
| `GET /api/tournament?teams=Spain,France,...` | title probabilities |

---

## How to Submit a PR

1. Create a branch from main

```bash
git checkout -b your-feature-name
```

2. Make your changes

3. Test that the project still builds

```bash
# In app/
cargo build --release

# In server/
cargo run --release
```

4. Open a PR against `main` with a clear description of what you changed and why

5. Tag `@gordian-engine` or `@victorchimakanu` for review

---

## Questions?

Drop a message in the `#builders` channel on the Aomi Discord. We answer everything.
