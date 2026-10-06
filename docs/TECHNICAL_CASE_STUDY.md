# Technical Case Study

## Problem and approach

Building a coding platform requires solving three hard problems: accepting user code, executing it safely, and keeping users engaged. Code-Spark combines a Django REST backend, async execution via Celery + Judge0, and a gamification layer (XP, levels, streaks, badges).

## Core architecture

**REST API.** Django REST Framework with JWT authentication. Custom email-based User model integrates with django-allauth (Google, GitHub). Viewsets handle CRUD and custom actions (`/api/v1/challenges/{id}/submit`). Token validation guards all endpoints.

**Async execution.** Submissions do not block. The API enqueues a Celery task and returns immediately. The task polls Judge0, updates the database with results (passed/failed/error) and XP earned, and retries on failure. The frontend polls for status or waits for WebSocket notification.

**Gamification.** Solving a challenge awards XP (difficulty-scaled). XP accumulates toward levels. UserProgress tracks streaks (consecutive submission days, reset if a day is missed), daily goals (user-set target), and badges (unlocked at milestones). No separate engagement engine; the schema enforces the rules.

## Key decisions

**Monolithic Django.** One codebase, one database, one Docker image simplifies deployment. App structure (by domain: accounts, challenges, progress, executor) enables clean separation without inter-service overhead.

**Judge0 managed API.** No need to build a sandboxing service. Judge0 handles many languages, resource limits, and logging. Trade-off: external dependency, but safer and faster than rolling custom.

**Redis for cache and broker.** One Redis instance handles session storage (via django-cache-framework) and Celery tasks. Simple setup, scalable if needed.

**Polling over WebSocket.** Channels are scaffolded but optional. Polling (frontend checks `/api/v1/submissions/{id}/` periodically) is sufficient when submissions run in seconds.

## Tradeoffs

**Judge0 bottleneck.** All submissions funnel through Judge0. If the API is down, the platform stalls. Mitigation: retry logic and Celery queue. At scale: cache hot problem results or use RabbitMQ.

**Flat directory layout.** The repo must live under a parent directory `codespark/`. Unconventional for Django; standard practice nests apps under a project root. Portfolio uses symlinks and PYTHONPATH tweaks.

**Schema-driven gamification.** XP, levels, badges are hardcoded in models. Changing rewards requires a migration. Alternative: store rules in a config table (trades simplicity for flexibility).

## Performance

Current measures: some queries use `select_related()`, list endpoints paginate at 20 results, Redis is available for caching, and static assets (Monaco, Bootstrap, jQuery) load from CDN. No load testing has been performed.

Possible scaling paths (not implemented or tested): vertical scaling, multiple gunicorn workers behind a load balancer, and Postgres read replicas.

## Deployment

Docker and docker-compose orchestrate locally: Django, Celery, Redis, nginx. Configuration is by environment variables (database URL, Redis URL, Judge0 endpoint), and a health endpoint is included.

Continue with [Engineering Challenges](ENGINEERING_CHALLENGES.md).
