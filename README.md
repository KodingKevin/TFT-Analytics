# TFT Analytics

Board comparison and meta analytics for Teamfight Tactics.

## Overview

Build two TFT boards — yours and an opponent's — and get an estimated
relative strength breakdown, grounded in unit quality, star levels, items,
active traits, board cost, and (eventually) historical match data.

## Status

This repo currently implements versions 0.1–0.4 from the project plan:

- Two interactive 7x4 hex boards (place, star up, item, remove units)
- The v0 board-strength scoring engine
- A Next.js API route (`POST /api/analyze`) exposing that engine
- The PostgreSQL schema the data pipeline will write into (not yet wired up)

Everything under "Later" in the roadmap below (Riot API import, the
analytics dashboard, comp clustering, and the Python/ML pipeline) is
scaffolded as stubs or schema only — see `scripts/` and `database/`.

## Features

- Click-to-place champions, star levels 1-3, up to 3 items per unit
- Estimated Board Strength with a factor-by-factor breakdown
- "Why favored" callouts per board
- Meta comp similarity match against a small placeholder comp list

## Tech stack

| Technology | Purpose |
|---|---|
| React / Next.js / TypeScript | Frontend + API routes |
| PostgreSQL | Persistent TFT data (schema only for now) |
| Riot API | Historical match data (stubbed in `scripts/`) |
| Data Dragon | Static champion/item/trait data (stubbed in `scripts/`) |
| Python / pandas / scikit-learn | Later: offline analytics and ML |

## Architecture

```
React (Next.js) -> REST API (Next.js route handlers) -> PostgreSQL
                                                             ^
                                                             |
                                          Riot TFT API + Data Dragon
                                             (via scripts/ ETL)
```

## Database schema

See `database/schema.sql`. Tables: `patches`, `champions`, `items`,
`traits`, `matches`, `participants`, `participant_units`, `unit_items`,
`participant_traits`.

## Data pipeline

`scripts/importChampions.ts` and `scripts/collectMatches.ts` are stubbed
ETL entry points for static data and match history respectively. Both
need `RIOT_API_KEY` and `DATABASE_URL` set (see `.env.example`) and are
not run automatically by this scaffold.

## Board-strength algorithm

Implemented in `web/lib/scoring.ts`:

```
Board Score = Unit Quality
            + Upgrade Quality
            + Item Strength
            + Trait Strength
            + Historical Comp Strength
```

This is an estimate, not a win probability — see Limitations.

## Meta detection algorithm

`bestCompMatch` in `web/lib/scoring.ts` compares a board's champions
against a small hand-authored comp list (`web/lib/data/comps.ts`) using
weighted set overlap. The plan's later version replaces this hand-authored
list with clustering over real match data.

## API endpoints

- `POST /api/analyze` — body `{ boardA: Board, boardB: Board }`, returns
  the full score breakdown and comparison for both boards.

Planned but not yet implemented: `/api/champions`, `/api/items`,
`/api/traits`, `/api/comps`, `/api/meta`, `/api/patches`.

## Testing

`web/tests/scoring.test.ts` covers the scoring engine (star-level ordering,
item scoring, unknown-champion handling, and comparison favoring). Run with
`npm test` from `web/`.

## Limitations

- Scores are an estimate of relative board strength, not a calibrated win
  probability — there is no labeled matchup dataset behind them yet.
- Champion/item/trait data is placeholder data, not real Data Dragon output.
- The comp list is hand-authored, not derived from clustering.

## Future improvements

Full roadmap in `docs/methodology.md`; short version: Riot static-data
import, real match collection, the analytics/meta-explorer dashboard,
comp clustering, and a Python-based ML strength model.

## Local setup

```bash
cd web
cp ../.env.example .env.local   # fill in RIOT_API_KEY / DATABASE_URL when ready
npm install
npm run dev
```

Open http://localhost:3000/analyzer.

To set up the database once you have a PostgreSQL instance:

```bash
psql $DATABASE_URL -f database/schema.sql
```

## Riot disclaimer

TFT Analytics isn't endorsed by Riot Games and doesn't reflect the views
or opinions of Riot Games or anyone officially involved in producing or
managing Riot Games properties. Riot Games and all associated properties
are trademarks or registered trademarks of Riot Games, Inc.

This project avoids real-time in-game features (live opponent scouting,
move-by-move guidance) by design — see `docs/methodology.md` for why.
