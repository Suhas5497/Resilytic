# Resilytics

<<<<<<< HEAD
Resilytics is a sales decision-intelligence SaaS platform for founders, CFOs, and strategy teams.

It combines:
- guided data onboarding
- dataset trust and ingestion diagnostics
- executive health and risk analysis
- scenario and shock simulation
- board-ready reporting
- role-aware product workflows

It is meant to help teams decide what to do next from sales and revenue data, not replace a BI dashboard with another dashboard.

The repository is a full-stack application with a FastAPI backend and a Vite/React frontend.

## Product Overview

Resilytics is designed around a Decision OS workflow:

1. Connect or upload company data
2. Validate and model the dataset
3. Review business health and risk drivers
4. Run shock and scenario analysis
5. Export decision-ready reports

The current launch posture is a founder-led soft launch for CSV-first SMB and light mid-market design partners, not a broad enterprise rollout.

## Tech Stack

### Backend
- FastAPI
- Python
- PostgreSQL
- Redis
- RQ workers
- DuckDB analytics warehouse
- SQLAlchemy

### Frontend
- React
- TypeScript
- Vite
- Tailwind CSS
- React Query
- React Router
- Recharts

## Core Capabilities

- Zero-data onboarding with sample and live-data paths
- CSV / Excel upload workflow with schema preview and validation
- Connector onboarding with lifecycle-aware sync feedback
- Dataset versioning with ingestion transparency and rejected-row diagnostics
- Multi-tenant warehouse access with SQL-layer tenant enforcement
- Executive Decision OS surfaces:
  - Business Health
  - Risk Drivers
  - Shock Simulator
  - Decision Scenarios
  - Reports
- Export and print support for executive reporting
- Demo-state preparation and tenant reset tooling

## Repository Structure

```text
.
|- backend/        FastAPI app, workers, migrations, tests
|- frontend/       React + TypeScript UI
|- docs/           Runbooks, release checklists, demo prep docs
|- scripts/        Local dev startup / shutdown helpers
|- .env.example    Backend + infrastructure environment template
|- docker-compose.yml
|- docker-compose.streaming.yml
```

## Prerequisites

Before running locally, make sure you have:

- Python 3.12+
- Node.js and npm
- Docker Desktop
- PowerShell

## Environment Setup

Create local environment files before starting the app:

1. Copy root env:

```powershell
Copy-Item .env.example .env
```

2. Copy frontend env:

```powershell
Copy-Item .\frontend\.env.example .\frontend\.env
```

3. Fill in required values in `.env`.

Important:
- Never commit real secrets
- Use `.env.example` and `frontend/.env.example` as templates only
- Local standalone defaults use dedicated ports so Resilytics can coexist with older local stacks:
  - Postgres `5433`
  - Redis `6380`
  - Backend API `8001`
  - Frontend dev server `5174`

## Recommended Local Development Command

From the repository root:

```powershell
powershell -ExecutionPolicy Bypass -File .\scripts\dev-start.ps1
```

This will:
- validate the local environment
- start Postgres and Redis with Docker
- run database migrations
- start the backend API
- start the queue worker
- start the frontend dev server

Useful companion commands:

```powershell
powershell -ExecutionPolicy Bypass -File .\scripts\dev-check.ps1
powershell -ExecutionPolicy Bypass -File .\scripts\dev-start.ps1 -CheckOnly
powershell -ExecutionPolicy Bypass -File .\scripts\dev-stop.ps1
```

To stop infrastructure as well:

```powershell
powershell -ExecutionPolicy Bypass -File .\scripts\dev-stop.ps1 -StopInfra
```

## Manual Local Startup

If you prefer to start services manually:

### 1. Start infrastructure

```powershell
docker compose up -d db redis
```

If event streaming is enabled:

```powershell
docker compose -f docker-compose.yml -f docker-compose.streaming.yml up -d db redis redpanda
```

### 2. Run migrations

```powershell
cd backend
..\.venv\Scripts\python.exe -m app.migrations.cli migrate
```

### 3. Start backend API

```powershell
..\.venv\Scripts\python.exe -m uvicorn app.main:app --host 0.0.0.0 --port 8001 --reload
```

### 4. Start worker

```powershell
..\.venv\Scripts\python.exe -m app.workers.run_worker
```

### 5. Start frontend

```powershell
cd ..\frontend
npm run dev
```

## Health Checks

Backend:

```powershell
Invoke-WebRequest http://127.0.0.1:8001/health/live -UseBasicParsing
```

Frontend:
- open `http://127.0.0.1:5174`

## Database Migrations

Resilytics uses an explicit migration workflow.

Run migrations manually with:

```powershell
cd backend
..\.venv\Scripts\python.exe -m app.migrations.cli migrate
```

Check schema status with:

```powershell
..\.venv\Scripts\python.exe -m app.migrations.cli status
```

The API verifies schema compatibility at startup but does not automatically mutate schema in production.

## Testing and Build Validation

### Backend tests

```powershell
cd backend
..\.venv\Scripts\python.exe -m pytest tests -q
```

### Frontend lint

```powershell
cd frontend
npm run lint
```

### Frontend production build

```powershell
npm run build
```

### Release check

```powershell
cd backend
..\.venv\Scripts\python.exe .\scripts\release_check.py
```

## Demo Preparation

Resilytics includes tenant-scoped demo reset tooling so repeated demos do not depend on manual cleanup.

Examples:

```powershell
cd backend
..\.venv\Scripts\python.exe .\scripts\demo_state.py list-companies
..\.venv\Scripts\python.exe .\scripts\demo_state.py reset-demo-tenant --company-id 1
..\.venv\Scripts\python.exe .\scripts\demo_state.py prepare-demo-state --company-id 1 --preset zero-data
..\.venv\Scripts\python.exe .\scripts\demo_state.py prepare-demo-state --company-id 1 --preset sample-data
..\.venv\Scripts\python.exe .\scripts\demo_state.py prepare-demo-state --company-id 1 --preset live-data --file "E:\data\demo.csv"
```

See:
- [docs/DEMO_PREPARATION.md](docs/DEMO_PREPARATION.md)

## Documentation

Useful docs in this repository:

- [docs/RUNBOOK_Step_By_Step.md](docs/RUNBOOK_Step_By_Step.md)
- [docs/RELEASE_READINESS_CHECKLIST.md](docs/RELEASE_READINESS_CHECKLIST.md)
- [docs/DEMO_PREPARATION.md](docs/DEMO_PREPARATION.md)

## Optional Assistant / LLM Configuration

The platform can run without OpenAI.

If you want deterministic-only behavior:

```bash
LLM_ENABLED=false
```

If you enable OpenAI locally, configure the relevant env values in `.env`.

## Current Product State

The platform is strongest in a controlled launch scope:

- core workflows are implemented
- onboarding and data-state handling are explicit
- dataset trust, provenance, and rejection transparency are visible
- async job infrastructure is in place
- Decision OS pages are structured and role-aware

Remaining work is concentrated in operational hardening, browser validation, and real-user pilot proof rather than missing core architecture.

## Contributing

When making changes:

- keep secrets out of version control
- run backend tests after backend changes
- run frontend lint and build after frontend changes
- prefer extending existing services and workflows instead of creating parallel subsystems
- preserve data-state, trust, and tenant-isolation behavior
=======
Resilytics is a Decision Intelligence platform that transforms raw business data into actionable strategic insights using a structured pipeline of ingestion, modeling, simulation, and decision analysis.

The system is designed to help founders, analysts, and executives understand business health, identify risk drivers, simulate scenarios, and make better decisions.

---

## Demo Preview

Demo Video:https://drive.google.com/file/d/1vxAe692qf56R729f54DHMTx1ZgMvfiDG/view?usp=sharing

The demo covers:

* onboarding and data upload
* dataset validation and modeling
* Business Health overview
* Risk Drivers analysis
* Shock Simulation
* Decision Scenarios
* executive reporting

---

## Product Overview

Resilytics follows a structured decision workflow:

Data → Insight → Risk Drivers → Simulation → Decision

Core modules:

* Business Health
  Executive-level summary of company performance and structural stability

* Risk Drivers
  Identification of key factors impacting performance and risk

* Shock Simulator
  Simulation of external shocks and downside scenarios

* Decision Scenarios
  Strategy comparison and outcome modeling

* Reports
  Boardroom-ready summaries and decision outputs

---

## Architecture Overview

Resilytics is built as a modular monolith with clear service boundaries.

Backend:

* FastAPI for API layer
* PostgreSQL for transactional storage
* Redis for queue and caching
* RQ workers for async processing
* DuckDB for analytical workloads

Frontend:

* React with TypeScript
* Vite build system
* React Router for navigation
* React Query for data fetching

Core services:

* ingestion pipeline
* canonical schema mapping
* warehouse service
* simulation engine
* async job orchestration

---

## Key Features

* Structured data ingestion with validation and schema detection
* Dataset versioning with full transparency and rejection tracking
* Multi-tenant architecture with tenant-safe query enforcement
* Monte Carlo simulation and forecasting engine
* Async job execution for heavy workloads
* Decision-focused UI (Decision OS)
* Exportable executive reports

---

## Data Pipeline

Resilytics processes data through the following stages:

1. Upload or connect data source
2. Schema detection and mapping
3. Canonical transformation
4. Modeling and feature generation
5. Warehouse sync
6. Decision intelligence generation

The system provides:

* ingestion stage counts
* rejection reasons
* modeling coverage metrics
* trust status indicators

---

## Project Structure

backend/

* api/
* services/
* models/
* workers/
* warehouse/
* ingestion/

frontend/

* src/
* components/
* pages/
* app/

infrastructure/

* docker-compose
* environment configuration

---

## Getting Started

### Prerequisites

* Python 3.10+
* Node.js 18+
* PostgreSQL
* Redis

---

### Backend Setup

cd backend

pip install -r requirements.txt

alembic upgrade head

uvicorn app.main:app --reload

---

### Worker

rq worker

---

### Frontend Setup

cd frontend

npm install

npm run dev

---

## Environment Variables

Configure the following:

DATABASE_URL=your_postgres_url
REDIS_URL=your_redis_url

Additional variables may be required depending on environment setup.

---

## Running with Docker

docker-compose up --build

This will start:

* API server
* PostgreSQL
* Redis
* workers

---

## Testing

Backend tests:

pytest

Frontend checks:

npm run lint
npm run build

---

## Current Status

Resilytics is a production-ready startup beta with:

* stable ingestion pipeline
* async job execution
* simulation engine
* decision-oriented UI

Ongoing improvements focus on:

* ingestion scalability
* performance optimization
* operational observability
* production hardening

---

## Roadmap

* streaming ingestion for large datasets
* improved warehouse scalability
* enhanced observability and monitoring
* frontend performance optimization
* expanded simulation governance

---

## Use Cases

* business performance analysis
* risk identification
* scenario planning
* strategic decision support
* executive reporting

---
>>>>>>> origin/main

