# AmakaFlow Dev Workspace

Workout content transformation and AI coaching platform.

## Quick Start

```bash
# 1. Setup (creates .env, builds images)
make setup

# 2. Edit .env with your API keys
nano .env

# 3. Start all services
make start

# 4. View logs
make logs
```

## Services

| Port | Service | Description |
|------|---------|-------------|
| 3000 | **UI** | React/Vite frontend |
| 8005 | **Chat API** | AI conversation service |
| 8001 | **Mapper API** | Exercise mapping & normalization |
| 8003 | **Calendar API** | Calendar integration |
| 8004 | **Workout Ingestor** | Video/OCR/text parsing |

## Project Structure

```
amakaflow-dev-workspace/
├── docker-compose.yml          # Service orchestration
├── docker-compose.override.yml # Dev overrides (hot reload)
├── .env                        # Environment vars (gitignored)
├── .env.example                # Template for .env
├── Makefile                    # Dev commands
│
├── amakaflow-ui/               # Frontend (React/Vite)
├── chat-api/                   # AI chat service (FastAPI)
├── mapper-api/                 # Exercise mapping (FastAPI)
├── calendar-api/               # Calendar service (FastAPI)
├── workout-ingestor-api/       # Video/OCR ingestion (FastAPI)
│
├── amakaflow-db/               # Database migrations (separate repo)
├── amakaflow-fitfiletool/      # Shared Python package
│
└── .claude/                    # Claude Code configuration
    ├── agents/                 # Custom AI agents
    └── skills/                 # Development skills
```

## Development Commands

```bash
# Full stack
make start      # Start all services
make stop       # Stop all services
make restart    # Restart all services
make status     # Show service status
make logs       # View all logs
make clean      # Remove containers and volumes

# Individual services
make ui         # Start only UI
make chat       # Start only chat-api
make mapper     # Start only mapper-api
make calendar   # Start only calendar-api
make ingestor   # Start only workout-ingestor

# Per-service logs
make logs-ui
make logs-chat
make logs-mapper
make logs-calendar
make logs-ingestor

# Rebuild
make build      # Rebuild all images
```

## Environment Variables

Copy `.env.example` to `.env` and configure:

**Required:**
- `SUPABASE_URL`, `SUPABASE_ANON_KEY`, `SUPABASE_SERVICE_ROLE_KEY`
- `CLERK_PUBLISHABLE_KEY`, `CLERK_SECRET_KEY`, `CLERK_DOMAIN`
- `OPENAI_API_KEY` and/or `ANTHROPIC_API_KEY`

**Optional:**
- `HELICONE_API_KEY` - AI observability
- `ELEVENLABS_API_KEY` - Text-to-speech
- `SENTRY_DSN` - Error tracking

See `.env.example` for full list with descriptions.

## Hot Reload

Development hot reload is enabled by default via `docker-compose.override.yml`:
- UI: Vite HMR
- APIs: Uvicorn `--reload`

Changes to source files automatically reload without container restart.

## Updating Services

```bash
# Update and rebuild a specific service
cd chat-api && git pull && cd ..
docker compose build chat
docker compose restart chat
```

## Related Repositories

- [amakaflow-db](https://github.com/supergeri/amakaflow-db) - Database migrations
- [amakaflow-fitfiletool](https://github.com/supergeri/amakaflow-fitfiletool) - FIT file utilities
