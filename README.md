<div align="center">

# Code-Spark

**A gamified coding-practice platform: solve challenges in a browser editor, earn XP, build streaks and climb a leaderboard.**

Python · Django · Django REST Framework · Celery · Redis · Judge0 · Bootstrap

![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white)
![Django](https://img.shields.io/badge/Django%204.2-092E20?logo=django&logoColor=white)
![DRF](https://img.shields.io/badge/Django%20REST%20Framework-A30000)
![Celery](https://img.shields.io/badge/Celery-37814A?logo=celery&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?logo=redis&logoColor=white)
![Bootstrap](https://img.shields.io/badge/Bootstrap%205-7952B3?logo=bootstrap&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)

<table>
  <tr>
    <td><img src="assets/screenshots/code-spark-dashboard.webp" width="440" alt="Dashboard with level banner, stat cards, daily goal progress bars and recent activity" /></td>
    <td><img src="assets/screenshots/code-spark-challenges.webp" width="440" alt="Challenge list with category and difficulty filters and challenge cards" /></td>
  </tr>
  <tr>
    <td><img src="assets/screenshots/code-spark-leaderboard.webp" width="440" alt="Leaderboard with podium for the top three and the current user's position" /></td>
    <td><img src="assets/screenshots/code-spark-code-editor.webp" width="440" alt="Code editor with language and theme selectors, templates and keyboard shortcuts" /></td>
  </tr>
</table>
<sub>Screenshots are real captures of a local build running on fictional demo data.</sub>

</div>

---

## Product at a Glance

| | |
|---|---|
| **What it is** | Coding-practice platform in the spirit of a lightweight LeetCode, with a progression layer |
| **Users** | Learners solving programming challenges; admins manage content through Django admin |
| **Gamification** | XP, levels, streaks, badges and achievements, daily goals, leaderboard |
| **Execution** | Code is submitted to a Judge0 service through a Celery task |
| **Auth** | Email accounts, optional Google and GitHub sign-in (django-allauth), JWT for API calls |
| **Data** | SQLite by default, Postgres through `DATABASE_URL`; Redis for cache and Celery broker |

## Product Showcase

<table>
  <tr>
    <td align="center"><img src="assets/screenshots/code-spark-challenge-detail.webp" width="300" alt="Challenge detail with problem statement, sample input and output, hints and recent submissions"/><br/><sub><b>Challenge detail</b><br/>Problem, samples, hints, history</sub></td>
    <td align="center"><img src="assets/screenshots/code-spark-achievements.webp" width="300" alt="Profile achievements tab showing earned and in-progress badges"/><br/><sub><b>Achievements</b><br/>Earned and in-progress badges</sub></td>
    <td align="center"><img src="assets/screenshots/code-spark-dashboard.webp" width="300" alt="Dashboard with daily goals"/><br/><sub><b>Dashboard</b><br/>Level, streak, daily goals</sub></td>
  </tr>
</table>

## Key Features

- **Challenge library.** Categories, difficulty levels, search and filters, a detail page with hints and recent submissions.
- **Browser code editor.** Monaco-based editor with language and theme selection and templates.
- **Asynchronous execution.** Submissions are queued; a Celery task sends code to Judge0 and stores the result.
- **Progression.** XP and levels, solve streaks, badges, achievements and optional daily goals.
- **Leaderboard.** Ranked users with a podium and a "your position" card.
- **Profile.** Stats, recent submissions, achievements and settings.
- **Admin.** Django admin for challenges, users, badges and execution records.

## Architecture

Server-rendered Django templates call a DRF API with a JWT bearer token. Four apps (accounts, challenges, progress, executor) separate the domains. Execution is handed to Celery, which talks to Judge0 and updates the database.

```mermaid
flowchart LR
    B[Browser templates + Monaco] --> API[Django + DRF]
    API --> DB[(SQLite / Postgres)]
    API --> R[(Redis)]
    API --> C[Celery worker]
    C --> J[Judge0]
```

Details: [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md).

## Engineering Highlights

- Domain-separated Django apps: accounts, challenges, progress, executor.
- Gamification modelled in the schema (XP, level, streak, badges, daily goals) rather than a separate engine.
- Execution queue and retry models for Judge0 submissions, with Celery tasks handling polling.
- Combined session and JWT authentication, with allauth for social sign-in.
- Docker, nginx and gunicorn configuration files and a health endpoint.

## Status & Scope

- The screenshots come from a local demo build seeded with fictional users, challenges and submissions; a one-click demo login is enabled only by an environment flag.
- Running the demo required small fixes to mismatches between views, serializers and models, plus a few missing front-end assets.
- **Run Code was not exercised in the demo:** it needs a Judge0 service, which was not available, so no execution results are shown.
- The WebSocket layer (Channels) is scaffolded but not active. There is no AI or LLM feature.
- No load testing has been performed.

## Documentation

| Read | For |
|---|---|
| [Technical case study](docs/TECHNICAL_CASE_STUDY.md) | End-to-end write-up |
| [Architecture](docs/ARCHITECTURE.md) | Apps, models and flows |
| [Engineering challenges](docs/ENGINEERING_CHALLENGES.md) | Problems, approaches, trade-offs |
| [Product overview](docs/PRODUCT_OVERVIEW.md) | Features and user flow |
| [Privacy and source](docs/PRIVACY_AND_SOURCE.md) | What is and is not published |

## Source Code

This repository is a portfolio showcase. Production source code is maintained privately. See [RIGHTS.md](RIGHTS.md) for usage terms.
