# Privacy and Source

## Source code

Production source code is private and not included in this repository. This portfolio contains analysis, diagrams, and documentation only.

## Demo data

All data shown (user profiles, leaderboard, submissions, badges) uses fictional demo data. No real user information is included.

When run locally with `CODESPARK_DEMO=1`, a seed script populates the database with five @example.com users, twelve sample challenges, and seeded progress history. This allows demonstration without exposing actual users.

## Secrets and configuration

Secrets are never stored in the repository:
- Django SECRET_KEY
- Database URL (Postgres credentials)
- Redis URL
- Judge0 API key
- Google and GitHub OAuth credentials
- Email service credentials

All are provided at runtime via environment variables. `.env*` files are excluded by `.gitignore`.

## Third-party services and licenses

Code-Spark uses:

- **Judge0** (compiles and executes code). See judge0.com/license.
- **Django, DRF, SimpleJWT, django-allauth, Celery, Redis.** Open source (MIT, BSD, Apache).
- **Bootstrap 5, Monaco Editor, jQuery.** MIT license.

All dependencies are listed in `requirements.txt` with versions.

## Production deployment (recommendations, not features of this project)

Operators should consider:
- Use managed database (AWS RDS, GCP SQL) with encrypted backups.
- Enable HTTPS and TLS.
- Use managed cache (AWS ElastiCache, GCP Memorystore).
- Store secrets in a vault (AWS Secrets Manager, Google Secret Manager).
- Enable audit logging.
- Update dependencies regularly for security patches.
- Implement rate limiting and DDoS protection (Cloudflare, AWS WAF).
- Isolate Judge0 endpoint from public internet if possible.
