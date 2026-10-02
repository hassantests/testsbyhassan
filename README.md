# AI Compliance Tool

An AI-powered compliance auditing platform that automates evidence management, compliance assessment against frameworks like ISO 27001 and NCA-ECC, and risk scoring.

## Overview

The AI Compliance Tool helps organizations:
- **Upload and manage compliance evidence** (policies, screenshots, reports)
- **Run AI-powered audits** against compliance frameworks
- **Identify and score risks** from non-compliant controls
- **Integrate with GitHub** for security posture audits

## Architecture

```
ai-compliance-tool/
├── server/          # FastAPI backend (Python)
│   └── app/
│       ├── api/      # REST API endpoints
│       ├── core/     # Config, JWT, security
│       ├── db/       # PostgreSQL + SQLAlchemy
│       └── scripts/  # DB initialization
├── ui/              # Next.js frontend (React + TypeScript)
└── reports/         # Generated audit reports
```

## Tech Stack

| Layer | Technology |
|-------|------------|
| Backend | FastAPI, Python 3.11+ |
| Frontend | Next.js 16, React 19, Tailwind CSS 4 |
| Database | PostgreSQL, SQLAlchemy |
| Vector Store | Chroma |
| AI/LLM | OpenAI GPT, LangChain |
| Auth | JWT (python-jose), Argon2 |

## Getting Started

### Prerequisites

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) installed and running
- An [OpenAI API key](https://platform.openai.com/api-keys)
- A GitHub App private key (for GitHub integration)

### Setup

1. **Clone the repository**
   ```bash
   git clone <repo-url>
   cd ai-compliance-tool
   ```

2. **Configure environment variables**

   Open `server/.env` and set your OpenAI API key:
   ```env
   OPENAI_API_KEY=sk-your-key-here
   ```

3. **Add GitHub App private key** (required for GitHub integration)

   Place your GitHub App private key file in the server directory:
   ```bash
   cp /path/to/your/private-key.pem server/private-key.pem
   ```
   This file is mounted read-only into the Docker container. The `PRIVATE_KEY_PEM` path in `.env` is already configured to `./private-key.pem`.

   > If you don't need GitHub integration, you can skip this step.

4. **Start the application**
   ```bash
   docker compose up --build
   ```

   This will automatically:
   - Start a PostgreSQL database
   - Run database migrations and seed test data
   - Start the FastAPI backend server
   - Start the Next.js frontend

5. **Access the application**

   | Service | URL |
   |---------|-----|
   | Frontend | http://localhost:3000 |
   | Backend API | http://localhost:8080 |
   | API Docs (Swagger) | http://localhost:8080/docs |

6. **Log in with test credentials**
   - **Email:** `test@example.com`
   - **Password:** `testpassword123`

### Useful Commands

| Command | Description |
|---------|-------------|
| `docker compose up --build` | Build and start all services |
| `docker compose up` | Start services (without rebuilding) |
| `docker compose down` | Stop all services |
| `docker compose down -v` | Stop and **delete all data** (database, uploads) |
| `docker compose logs server` | View server logs |
| `docker compose logs ui` | View UI logs |

### Development

Code changes hot-reload automatically:
- Edit files in `server/` — the backend restarts via uvicorn `--reload`
- Edit files in `ui/src/` — the frontend rebuilds via Next.js Turbopack

### Running Without Docker

If you prefer to run services directly:

**Server:**
```bash
cd server
python -m venv .venv
source .venv/bin/activate        # Linux/macOS
# .venv\Scripts\activate         # Windows
pip install -r requirements.txt
python -m app.scripts.setup_db
uvicorn app.main:app --host 0.0.0.0 --port 8080 --reload
```

**UI:**
```bash
cd ui
npm install
npm run dev
```

> Note: This requires PostgreSQL running locally. See `server/.env` for database connection settings.

## Contributing

1. Fetch latest from release branch
2. Create feature branch: `git checkout -b feature/your-feature`
3. Make changes and commit
4. Push and open Pull Request to release branch

> **Do not push directly to `main` or `release/*` branches**
