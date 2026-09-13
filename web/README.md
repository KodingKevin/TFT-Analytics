# TFT Analytics — Web Application

The web application for **TFT Analytics**, a Teamfight Tactics analytics platform for building and comparing boards, analyzing team strength, and eventually using historical match data to identify meta compositions and trends.

This application is built with **Next.js, React, TypeScript, and Tailwind CSS**.

## Features

Current and planned features include:

- Interactive TFT board builder
- Build your board and an opponent's board
- Champion selection and placement
- Star-level selection
- Item selection
- Estimated board-strength comparison
- Strength breakdowns based on:
  - Unit quality
  - Unit upgrades
  - Items
  - Active traits
  - Composition strength
- Meta composition matching
- Historical TFT analytics
- Champion and item statistics

> The board analyzer provides an **estimated board strength**, not an exact combat win probability.

## Tech Stack

- **Next.js** — Web application framework
- **React** — Interactive user interface
- **TypeScript** — Type-safe application development
- **Tailwind CSS** — Styling
- **Node.js** — Server-side/API logic
- **PostgreSQL** — Historical TFT data and analytics

Future versions may also use Python, pandas, and machine-learning libraries for offline data analysis and modeling.

## Getting Started

### Prerequisites

Install:

- Node.js
- npm

Verify your installation:

```bash
node --version
npm --version
```

### Install Dependencies

From the `web` directory:

```bash
npm install
```

### Run the Development Server

```bash
npm run dev
```

Then open:

`http://localhost:3000`

The application will automatically update as source files are changed.

## Project Structure

```text
web/
├── app/
│   ├── page.tsx
│   ├── analyzer/
│   └── api/
│
├── components/
│   ├── Board/
│   ├── Champion/
│   ├── Items/
│   ├── Traits/
│   └── Analysis/
│
├── lib/
├── public/
├── types/
│
├── package.json
├── tsconfig.json
└── next.config.ts
```

## Environment Variables

Environment variables containing credentials or secrets should never be committed to GitHub.

An example configuration is provided in the repository's `.env.example` file.

Example:

```env
DATABASE_URL=your_database_url_here
RIOT_API_KEY=your_riot_api_key_here
```

Create your own local environment file when these services are added.

Do **not** place real credentials in `.env.example`.

## Testing

Run the automated tests with:

```bash
npm test
```

Tests will cover areas such as board scoring, board comparison, and API behavior as development continues.

## Development Roadmap

### Phase 1 — Board Analyzer

- Interactive hex board
- Champion placement
- Star levels
- Items
- Board comparison
- Estimated strength scoring

### Phase 2 — Database

- PostgreSQL integration
- Champion data
- Item data
- Trait data
- Match history storage

### Phase 3 — Riot Data

- Import TFT static data
- Collect historical match data
- Normalize and store match information

### Phase 4 — Analytics

- Champion statistics
- Item statistics
- Composition statistics
- Patch comparisons
- Meta composition detection

### Phase 5 — Advanced Analytics

- Historical board-strength modeling
- Composition clustering
- Machine-learning experiments
- Improved matchup analysis

## Disclaimer

TFT Analytics is an independent project and is not affiliated with or endorsed by Riot Games.

Teamfight Tactics and Riot Games are trademarks or registered trademarks of Riot Games, Inc.