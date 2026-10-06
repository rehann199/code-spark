# Product Overview

## What it is

Code-Spark is a Django-powered coding-practice platform. Users solve challenges, write and execute code in the browser, and progress through a gamified system: XP, levels, streaks, badges, daily goals, and leaderboards.

## Core features

- **Challenges.** Browse problems by category and difficulty. Each includes statement, hints, starter code.
- **Code editor.** Monaco in-browser editor with syntax highlighting. Submit for execution against test cases.
- **Execution.** Judge0 API compiles and runs code. Results show pass/fail status and output.
- **Gamification.** Earn XP per submission (difficulty-scaled). Accumulate XP to level up. Maintain solve streaks (reset if a day is missed). Unlock badges at milestones. Set daily goals.
- **Dashboard.** Quick view: current level, XP, active streak, challenges solved, recent activity.
- **Profile.** Earned badges, total solved, XP breakdown, longest streak.
- **Leaderboard.** Global rankings by total XP.

## Authentication and user flow

Email sign-up or social sign-in (Google, GitHub) via django-allauth. JWT tokens stored in browser localStorage. Flow: Sign up → Dashboard → Browse challenges → Open editor → Write code → Submit → See results and XP → Track progress.

## Demo mode

Environment variable `CODESPARK_DEMO=1` enables one-click demo login with fictional user (email @example.com). Real sign-up and sign-in unchanged. Demo data: seeded users, 12 sample challenges, badges, submission history.
