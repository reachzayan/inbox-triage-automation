# Inbox Triage Automation

A shared-mailbox assistant that ingests email, classifies incoming messages, and suggests replies for human review. Built with FastAPI, Next.js, PostgreSQL with pgvector, Redis, Microsoft Graph, and a language model provider.

## Features

- Ingestion from inbox, junk, and sent folders through Microsoft Graph.
- Classification, context retrieval, and suggested replies.
- A dashboard for mailbox threads, urgency, and review notes.
- Authentication, background workers, and audit records.

Mailbox access is read-only (`Mail.Read`). A human sends final replies from their mail client.

## Layout

- `backend/`: API, mailbox integration, workers, database migrations, and tests.
- `frontend/web/`: dashboard and authentication screens.
- `frontend/demo/`: static design reference.
- `deploy/`: deployment helpers.

## Local setup

Install Docker with Compose, Node.js 20+, and configure a Microsoft Entra application with Microsoft Graph `Mail.Read` application permissions. Provide your own tenant, application credentials, model API key, and authentication secrets using the environment example.

```bash
cp backend/.env.example backend/.env
# Fill required settings with your own values.
docker compose up --build
```

The API runs at `http://localhost:8000`; health is available at `/health`. The local Compose stack includes PostgreSQL and Redis.

Run the dashboard separately:

```bash
cd frontend/web
npm ci
npm run dev
```

For local proxying, leave `NEXT_PUBLIC_API_BASE_URL` empty and use `BACKEND_PROXY_URL=http://localhost:8000` so the dashboard forwards API requests to the backend.

Create a local dashboard user:

```bash
docker compose exec backend python -m scripts.seed_user --email user@example.com --password 'replace-with-a-strong-local-password'
```

Production settings require your own HTTPS hosts, TLS Redis configuration, database credentials, and authentication secrets. See `backend/.env.example` and `docker-compose.prod.yml` for the available settings.
