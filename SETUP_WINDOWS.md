# Windows Setup Guide

## Prerequisites

- [Docker Desktop for Windows](https://www.docker.com/products/docker-desktop/) installed and running
- An [OpenAI API key](https://platform.openai.com/api-keys)

## Setup

1. **Clone the repository**
   ```powershell
   git clone <repo-url>
   cd ai-compliance-tool
   ```

2. **Configure environment variables**

   Open `server/.env` and set your OpenAI API key:
   ```env
   OPENAI_API_KEY=sk-your-key-here
   ```

3. **Add GitHub App private key** (required for GitHub integration)

   Place your `private-key.pem` file in the `server/` directory:
   ```powershell
   copy C:\path\to\your\private-key.pem server\private-key.pem
   ```

   > If you don't need GitHub integration, you can skip this step.

4. **Start the application**
   ```powershell
   docker compose up --build
   ```

5. **Access the application**

   | Service | URL |
   |---------|-----|
   | Frontend | http://localhost:3000 |
   | Backend API | http://localhost:8080 |
   | API Docs (Swagger) | http://localhost:8080/docs |

## Default Test Credentials

- **Email:** `test@example.com`
- **Password:** `testpassword123`

## Useful Commands

| Command | Description |
|---------|-------------|
| `docker compose up --build` | Build and start all services |
| `docker compose up` | Start services (without rebuilding) |
| `docker compose down` | Stop all services |
| `docker compose down -v` | Stop and **delete all data** (database, uploads) |
| `docker compose logs server` | View server logs |
| `docker compose logs ui` | View UI logs |

## Notes

- The database is created automatically on first run — no manual PostgreSQL install needed
- Code changes in `server/` and `ui/src/` hot-reload automatically
- Data persists across restarts. Use `docker compose down -v` to reset everything
