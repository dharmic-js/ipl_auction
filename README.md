# IPL AUCTION VALUE OPTIMIZER
### Data-Driven Sports Analytics, Bayesian Performance Index & 0-1 Knapsack MILP Squad Optimization

[![Python](https://img.shields.io/badge/Python-3.12%2B%20%7C%203.14-blue.svg)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.110%2B-009688.svg)](https://fastapi.tiangolo.com/)
[![React](https://img.shields.io/badge/React-19-61DAFB.svg)](https://react.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.0%2B-3178C6.svg)](https://www.typescriptlang.org/)
[![Vite](https://img.shields.io/badge/Vite-8.0%2B-646CFF.svg)](https://vitejs.dev/)
[![Tests](https://img.shields.io/badge/pytest-27%20passed-brightgreen.svg)](https://docs.pytest.org/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

## 1. What the Project Does

The **IPL Auction Value Optimizer** is a complete, production-grade, data-driven web application and sports analytics research platform. It analyzes delivery-level historical performances from all **1,243 Indian Premier League (IPL) matches spanning 19 seasons (2008 to 2026)**, reconciles those performances with **2,628 historical auction and retention records**, and optimizes squad selection under a configurable auction purse and composition constraints using mathematical programming.

Unlike static portfolio dashboards or toy projects, this application executes **real Python calculations**, parses **295,732 genuine delivery events**, enforces **strict MCC cricket laws**, applies **Bayesian era-normalized shrinkage**, and solves the multi-constrained **0-1 Knapsack problem** using SciPy's Mixed-Integer Linear Programming (MILP) HiGHS solver.

---

## 2. Why It Is Useful

Every IPL auction involves franchise executives allocating a ₹ 100+ Crore purse across 18 to 25 squad spots under intense time constraints. Common pitfalls include:
- Overpaying for marquee stars with declining or era-inflated baseline statistics.
- Underestimating domestic uncapped players who provide elite run-rate premiums at base prices.
- Failing to mathematically balance role constraints (batters, bowlers, all-rounders, wicketkeepers) and the overseas player cap (maximum 8 per squad).

This application provides:
- **Era-Adjusted Evaluation:** Normalizes batting strike rate and bowling economy against baseline tournament run rates across Early (2008–2013), Tactical (2014–2019), and Modern Hyper-Impact (2020–2026) eras.
- **Bayesian Sample Shrinkage:** Mitigates small-sample variance so that a player with a single 30-run cameo does not outrank a seasoned veteran.
- **Descriptive Price Efficiency:** Quantifies performance delivered per ₹ Crore paid (`overall_performance_index / sold_price_in_crore`).
- **Global Knapsack Optimality:** Replaces ad-hoc manual guesswork with an exact branch-and-cut optimization solver.

---

## 3. Main Features

- **Executive Analytics Dashboard:**
  - High-level KPIs: 1,243 matches, 964 canonical players, 2,628 auctions, 19 seasons.
  - Interactive Recharts Price-vs-Performance Frontier scatter chart.
  - Top 10 Performance Index leaders and Top 10 Price-Efficiency value bargains.
  - Era baseline evolution cards.
- **Player Explorer:**
  - Comprehensive table covering all 964 canonical players.
  - Multi-faceted filters: Role, Nationality (Indian/Overseas), Franchise Team, Minimum Score.
  - Instant debounced search and sorting by any analytical metric.
  - Detailed slide-out Player Profile Drawer featuring career records, radar breakdown, season progression, and auction transactions.
- **Player Comparison:**
  - Multi-player side-by-side comparison for 2 to 4 candidates.
  - Comparative bar/radar chart across 4 key dimensions.
  - Comprehensive metric differential table (Runs, Strike Rate, Average, Wickets, Economy, Dot %, 50s/100s, Cost).
- **Auction Lab & Squad Value Optimizer (Flagship Feature):**
  - Interactive purse slider (₹ 30 Cr to ₹ 150 Cr) and squad size selector (11 to 25 players).
  - Configurable overseas cap (up to 8 overseas players).
  - Role quota boundaries (Min/Max Batters, Bowlers, All-Rounders, WKs).
  - Pricing modes: Auto, Historical Sold Strictly, Market Power-Law Estimate, or Custom Scenario.
  - Marquee player retention / lock-in feature (optimizer solves around locked players).
  - SciPy HiGHS MILP 0-1 Knapsack Solver with deterministic greedy fallback.
  - Real-time purse breakdown, role distribution badges, and binding constraint analysis.
  - One-click CSV and JSON scenario export.
- **Methodology & Data Quality Page:**
  - Transparent mathematical formulas, Bayesian shrinkage weights, and MCC delivery rules.
  - Full diagnostic data quality audit report (extras audit, uncredited wickets, denominator zero-checks).
  - Academic Python concepts documentation.

---

## 4. Technology Stack

- **Data Engineering & Analytics:** Python 3.12+ / 3.14, Pandas, NumPy, SciPy (HiGHS MILP solver), PyArrow.
- **Backend API:** FastAPI, Pydantic v2, Uvicorn, HTTPX.
- **Frontend Application:** React 19, TypeScript, Vite, Recharts, Lucide Icons, Pure CSS Design System.
- **Testing & Quality Assurance:** Pytest (27 automated unit and integration tests).
- **Deployment & Hosting:** Git/GitHub monorepo structured for Vercel Serverless Functions (`api/index.py` ASGI handler) and static CDN frontend (`frontend/dist`).

---

## 5. Project Directory Structure

```text
ipl-auction-value-optimizer/
│
├── README.md                      # Comprehensive project documentation
├── AGENTS.md                      # Repository instructions and operating guidelines
├── .gitignore                     # Git exclusions (Python, Node, large raw files)
├── .env.example                   # Environment configuration template
├── requirements.txt               # Python backend and data dependencies
├── package.json                   # Root workspace scripts
├── vercel.json                    # Vercel serverless routing configuration
│
├── api/
│   └── index.py                   # Vercel Serverless Python ASGI entrypoint
│
├── data/
│   ├── raw/                       # Original read-only source files
│   │   ├── cricsheet/             # 1,243 Cricsheet IPL match JSON files
│   │   ├── people.csv             # Cricsheet global player registry (18,651 players)
│   │   └── auction/               # Outside-Edge IPL auction parquet & crosswalks
│   ├── interim/                   # Intermediate staged artifacts
│   ├── processed/                 # Compact reproducible production artifacts
│   │   ├── players.json           # 964 canonical players with scores & careers
│   │   ├── players.csv            # Tabular player export
│   │   ├── player_seasons.json    # 2,960 season-by-season player records
│   │   ├── auction_history.json   # 2,628 auction/retention transactions
│   │   ├── team_summaries.json    # 19 franchise historical records
│   │   ├── season_baselines.json  # 19 season baseline run-rate statistics
│   │   ├── metadata.json          # Dataset version and coverage metadata
│   │   └── manifest.json          # SHA-256 artifact verification manifest
│   └── reports/
│       ├── data_quality_report.json # Delivery, extras, and wicket audit report
│       └── exclusion_report.json    # Excluded records log
│
├── analytics/                     # Reusable Python analytical modules
│   ├── config.py                  # Metric weights, directories, and constants
│   ├── discover_files.py          # Input file integrity and discovery
│   ├── load_people.py             # Cricsheet people registry loader
│   ├── load_auction_data.py       # Auction normalizer (Lakhs to ₹ Crore)
│   ├── normalize_players.py       # Player reconciler, aliases, do_not_merge
│   ├── parse_cricsheet.py         # Deterministic parser for 1,243 match JSONs
│   ├── calculate_batting.py       # Batting metrics (wides, boundaries, averages)
│   ├── calculate_bowling.py       # Bowling metrics (legal balls, maidens, wickets)
│   ├── calculate_fielding.py      # Fielding catches, stumpings, run-outs
│   ├── calculate_match_metrics.py # Season baselines and franchise summaries
│   ├── calculate_player_scores.py # Bayesian shrinkage & era-normalized scores
│   ├── estimate_prices.py         # Convex power-law market price estimator
│   ├── optimize_squad.py          # 0-1 Knapsack MILP solver via SciPy HiGHS
│   ├── audit_data.py              # Diagnostic data auditor
│   ├── export_data.py             # Production artifact serializer
│   └── pipeline.py                # End-to-end pipeline orchestrator
│
├── backend/                       # FastAPI application
│   ├── app.py                     # Main FastAPI entrypoint with CORS & lifespan
│   ├── config.py                  # API settings and directory bindings
│   ├── schemas.py                 # Pydantic request/response validation schemas
│   ├── services/
│   │   └── data_service.py        # In-memory artifact loader and search engine
│   └── routers/
│       ├── players.py             # GET /api/players, GET /api/players/{id}
│       ├── rankings.py            # GET /api/rankings
│       ├── auctions.py            # GET /api/auctions, GET /api/auctions/years
│       ├── compare.py             # POST /api/compare
│       ├── optimizer.py           # POST /api/optimizer/run
│       └── metadata.py            # GET /api/metadata, GET /api/data-quality
│
├── frontend/                      # React + TypeScript + Vite web app
│   ├── package.json               # Frontend dependencies (React, Recharts, Lucide)
│   ├── vite.config.ts             # Vite configuration
│   ├── tsconfig.json              # Strict TypeScript configuration
│   ├── index.html                 # HTML shell with Google Fonts & metadata
│   ├── dist/                      # Compiled production build
│   └── src/
│       ├── App.tsx                # Main application component & router
│       ├── index.css              # Custom white-and-red design system CSS
│       ├── types/index.ts         # TypeScript analytical interfaces
│       ├── lib/api.ts             # API client methods
│       ├── components/
│       │   ├── Navbar.tsx         # Sticky header with navigation
│       │   └── PlayerDrawer.tsx   # Detailed player profile modal drawer
│       └── pages/
│           ├── Dashboard.tsx      # Overview, KPIs, frontier scatter chart
│           ├── PlayerExplorer.tsx # Filterable, sortable player catalog
│           ├── PlayerComparison.tsx # Side-by-side player comparison
│           ├── SquadBuilder.tsx   # Interactive 0-1 Knapsack Squad Optimizer
│           └── Methodology.tsx    # Mathematical formulas and audit reports
│
├── tests/                         # Automated Pytest suite (27 passing tests)
│   ├── test_batting_metrics.py    # Wides, no-balls, batting averages
│   ├── test_bowling_metrics.py    # Legal balls, uncredited wickets, economy
│   ├── test_auction_prices.py     # Currency normalization, unsold handling
│   ├── test_player_identity.py    # Hex IDs, aliases, do_not_merge constraints
│   ├── test_scoring.py            # Era normalization, Bayesian shrinkage
│   ├── test_optimizer.py          # MILP knapsack solver, budget enforcement
│   ├── test_data_loading.py       # Artifact schemas and integrity
│   └── test_backend_api.py        # FastAPI TestClient endpoint integration
│
├── docs/                          # Detailed technical documentation
│   ├── DATA_INVENTORY.md          # Raw dataset inventory, hashes, licensing
│   ├── DATA_QUALITY_REPORT.md     # Audit of delivery extras, wickets, prices
│   ├── DATA_DICTIONARY.md         # Formal metric definitions and formulas
│   ├── ANALYTICAL_METHODOLOGY.md  # Mathematical defense of models
│   ├── MODEL_LIMITATIONS.md       # Analytical limitations and disclosures
│   ├── ARCHITECTURE.md            # Decoupled system architecture
│   ├── DEPLOYMENT.md              # Local run & Vercel deployment instructions
│   └── PYTHON_CONCEPTS.md         # Academic concepts reference
│
└── scripts/                       # Executable CLI scripts
    ├── run_pipeline.py            # Runs the full analytics pipeline
    ├── run_tests.py               # Runs pytest test suite
    └── validate_deployment.py     # Pre-flight deployment check
```

---

## 6. Required Software

- **Python:** Version 3.12, 3.13, or 3.14.
- **Node.js:** Version 18.0.0 or higher (v24 LTS tested).
- **Package Managers:** `pip` and `npm`.
- **Git:** Version 2.20 or higher.

---

## 7. How to Create a Python Virtual Environment

```bash
# Navigate to the project root
cd ipl-auction-value-optimizer

# Create a virtual environment
python -m venv .venv

# Activate the virtual environment:
# Windows (PowerShell):
.venv\Scripts\Activate.ps1
# Windows (Command Prompt):
.venv\Scripts\activate.bat
# Linux / macOS:
source .venv/bin/activate
```

---

## 8. How to Install Backend Dependencies

```bash
pip install -r requirements.txt
```

---

## 9. How to Install Frontend Dependencies

```bash
cd frontend
npm install
cd ..
```

---

## 10. Where the Raw Data Files Belong

Raw data files reside under `data/raw/`:
- `data/raw/cricsheet/`: Contains the 1,243 Cricsheet JSON match files (`335982.json` to `1473550.json`).
- `data/raw/people.csv`: The canonical Cricsheet global player registry.
- `data/raw/auction/`: Contains `auction_all_years.parquet`, `name_aliases.csv`, and `do_not_merge.csv`.

> **Note:** The original raw match files are strictly read-only and preserved untouched.

---

## 11. How to Run the Python Pipeline

The analytics pipeline parses all 1,243 matches, computes era-normalized scores, audits data quality, and outputs production artifacts into `data/processed/` in ~18 seconds:

```bash
python scripts/run_pipeline.py
```

Generated production artifacts:
- `data/processed/players.json` (1.42 MB, 964 players)
- `data/processed/player_seasons.json` (3.45 MB, 2,960 season records)
- `data/processed/auction_history.json` (1.11 MB, 2,628 transactions)
- `data/processed/team_summaries.json` & `season_baselines.json`
- `data/reports/data_quality_report.json` & `exclusion_report.json`

---

## 12. How to Run the Tests

Execute the 27 unit and integration tests across data loading, identity, batting, bowling, auction prices, scoring, optimizer, and FastAPI routes:

```bash
python -m pytest -v tests/
```

Or using the runner script:
```bash
python scripts/run_tests.py
```

Expected output:
```text
======================== 27 passed in 2.21s ========================
```

---

## 13. How to Start the Frontend and Backend Locally

### Option A: Running Backend and Frontend Simultaneously

**Terminal 1 — Backend API (FastAPI):**
```bash
python -m uvicorn backend.app:app --reload --port 8000
```
*API will run at `http://127.0.0.1:8000` (Interactive docs: `http://127.0.0.1:8000/api/docs`)*

**Terminal 2 — Frontend App (Vite):**
```bash
cd frontend
npm run dev
```
*Frontend will run at `http://localhost:5173`*

---

## 14. How to Use Each Page

1. **Dashboard:**
   - Review overall tournament scale across 19 IPL seasons.
   - Inspect the Price vs Performance Scatter Frontier. Click any point to open that player's profile.
   - Examine the Top 10 Performance Index leaders and Top 10 Price Efficiency bargains.
2. **Player Explorer:**
   - Search by player name (e.g., "Bumrah", "Kohli", "Narine").
   - Filter by role (Batter, Bowler, All-Rounder, Wicketkeeper) or Overseas status.
   - Adjust the Min Performance Index slider to isolate elite performers.
   - Click **Inspect** to view a player's career breakdown, or click the **Scale** icon to add them to comparison.
3. **Player Comparison:**
   - Compare 2 to 4 candidates side-by-side.
   - Use the autocomplete input to add any player.
   - View the multi-bar chart and detailed metric comparison table.
4. **Auction Lab / Squad Builder:**
   - Set your total purse budget in ₹ Crore (e.g. ₹ 100 Cr).
   - Set squad size (e.g. 18 players) and max overseas limit (e.g. 8).
   - Configure role bounds (min/max batters, bowlers, all-rounders, wicketkeepers).
   - Pin specific marquee players into the squad using the "Locked Core Players" search.
   - Click **Solve Optimal Squad** to trigger the SciPy HiGHS 0-1 Knapsack solver.
   - Review the selected lineup, purse utilization, remaining budget, and binding constraints.
   - Export the squad as CSV or download the complete scenario as JSON.
5. **Methodology & Quality:**
   - Read the mathematical definitions, Bayesian shrinkage formula, and era baseline weights.
   - Inspect the live data pipeline audit report and cricket rule enforcement details.

---

## 15. How to Interpret the Scores

- **Overall Performance Index (0–100):** A Bayesian era-normalized composite score reflecting a player's historical contribution in their specific role.
  - **85.0 – 100.0 (Elite):** All-time IPL match-winners (e.g. Sunil Narine, Jasprit Bumrah, Shane Watson, Suryakumar Yadav).
  - **70.0 – 84.9 (High Impact):** Consistent tournament regulars and franchise cornerstones.
  - **50.0 – 69.9 (Solid Contributor):** Average league performers meeting baseline expectations.
  - **Below 50.0 (Low Impact / Shrunk):** Sub-par returns or players with very small career sample sizes shrunk towards the prior mean (50.0).
- **Price Efficiency (`overall_performance_index / sold_price_in_crore`):**
  - Measures points delivered per ₹ Crore spent. High efficiency values (e.g. 200–450 pts/Cr) represent historic bargain acquisitions (e.g. Suryakumar Yadav or Axar Patel at ₹ 0.20 Cr base prices).
  - Strictly computed for verified positive sold prices; never calculated for unsold or unpriced records.

---

## 16. Known Limitations

- **Retrospective Descriptive Nature:** Performance scores reflect historical observed contributions; cricket variance, team dynamics, pitch conditions, and injuries cannot be forecasted with certainty.
- **Auction Lot Sequencing:** The optimizer solves a simultaneous knapsack problem. Real-world IPL auctions occur in sequential lots where bidding wars and auction dynamics shift market valuations dynamically.
- **Unsold Players:** Unsold players are never assigned a price of zero. In cost-constrained optimizations, players without historical prices are either priced via market power-law estimates or excluded with explicit reasons.

---

## 17. How to Connect GitHub and Deploy to Vercel

### Pre-Flight Validation Check

Before committing or deploying, run the automated validator:
```bash
python scripts/validate_deployment.py
```
*Expected: `ALL DEPLOYMENT PRE-FLIGHT CHECKS PASSED — READY FOR VERCEL & GITHUB`*

### Publishing to GitHub

```bash
# Check git status
git status

# Add files (large raw archives and node_modules are already in .gitignore)
git add .

# Commit
git commit -m "feat: complete IPL Auction Value Optimizer with Python analytics and React frontend"

# Add your remote repository and push
git remote add origin https://github.com/<your-username>/ipl-auction-value-optimizer.git
git branch -M main
git push -u origin main
```

### Deploying on Vercel

1. Log in to [Vercel](https://vercel.com/) and click **"Add New Project"**.
2. Import your GitHub repository `ipl-auction-value-optimizer`.
3. Configure the project:
   - **Framework Preset:** Vite
   - **Root Directory:** `./`
   - **Build Command:** `npm run build:frontend`
   - **Output Directory:** `frontend/dist`
4. Set Environment Variables:
   - `ALLOWED_ORIGINS`: `https://<your-project-name>.vercel.app,http://localhost:5173`
   - `VITE_API_BASE_URL`: (Leave empty to use same-origin `/api`, or specify backend URL)
5. Click **Deploy**. Vercel will build the frontend assets and automatically route all `/api/*` calls to the Python serverless function at `api/index.py`.

---

## License & Attribution

- **Application Code:** [MIT License](LICENSE).
- **Match Deliveries Source:** [Cricsheet](https://cricsheet.org/) licensed under [Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)](https://creativecommons.org/licenses/by-sa/4.0/).
- **Auction Data Source:** [outside-edge/ipl_auction](https://github.com/outside-edge/ipl_auction) licensed under the MIT License.
- **Disclaimer:** This project is an independent sports analytics tool and is not affiliated with, endorsed by, or sponsored by the Board of Control for Cricket in India (BCCI) or the Indian Premier League (IPL).
