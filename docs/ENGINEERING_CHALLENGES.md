# Engineering Challenges

## Async code execution

Judge0 compilation takes seconds. Rather than block the user, Celery workers poll asynchronously: submit returns immediately with a task ID; the task polls Judge0, updates the database, and the frontend checks for results. This decouples submission from execution latency and allows retries on failure.

## Level and streak calculation

Streaks are fragile (miss one day, lose the streak). Storing a snapshot as a field risks inconsistency. Instead, UserProgress records `last_solved_date` and recalculates streak on each submission. Levels derive from cumulative User.xp (not stored). This makes the schema deterministic: any state can be reconstructed from submission history.

## Gamification without overload

Too many mechanics (XP, levels, streaks, badges, daily goals) overwhelm new users. Code-Spark layers them progressively: day one earn XP and level; day seven see a streak; day thirty unlock badges. Daily goals are optional. Dashboard and profile messaging guide the user, so all features exist but are not forced upfront.

## Multi-auth coordination

Supporting email, Google, and GitHub sign-in via django-allauth requires coordinating different flows and schemas. The solution: allauth handles provider flow and user creation; all sign-in paths converge to a JWT token stored in localStorage. CSRF is disabled for API endpoints (JWT replaces it) but active for template forms. Signals trigger token generation on login.

## Test case isolation

If one submission crashes Judge0 or results leak between submissions, trust erodes. Each submission gets a unique Judge0 token; requests are stateless (all context included). Results store status, stderr, stdout, and timing. Celery polls only that token (not a global queue) and retries on Judge0 failure.

## Query efficiency and caching

Leaderboard queries scan all users; challenge lists filter by category. Redis is available for caching. Some queries use `select_related()` to cut DB roundtrips. Pagination returns 20 results per page.

Continue with [Privacy and Source](PRIVACY_AND_SOURCE.md).
