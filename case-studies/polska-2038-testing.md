# Case study: test automation for Polska 2038

[Polska 2038](https://polska2038.pl) is a civic, pro bono web project I run: a 40+ page bilingual (PL/EN) site with interactive maps, budget dashboards, a survey and a small community section. Frontend in React 18 and Vite with server-side prerendering, backend in FastAPI, deployed on Vercel.

The repository is private because it also holds outreach material and deployment details. This page describes how it is tested. I am happy to walk through the code in an interview.

## How I work on it

The project is developed with AI coding assistants (Claude Code, ChatGPT). My part is the testing side: deciding what each check must catch, running the suites, reviewing every pull request and keeping a written decision log. Every change goes through a pull request; nothing reaches production without a green test run.

## Test suite

| Layer | Tooling | Size |
| --- | --- | --- |
| End-to-end | Playwright, two projects: desktop 1440×900 and phone 390×844 with touch | 126 test definitions, many parametrised over routes and widths; last full local run: **456 passed, 92 skipped, 0 failed, 0 flaky** (18.11.2026) |
| Frontend unit | Node test runner | 132 tests in 26 files |
| Backend API | pytest + httpx against FastAPI with a temporary SQLite database and captured e-mails | 12 tests: magic-link login, uploads with images, moderation, comments, reactions, rate limits |
| Static checks | ESLint, plus a report of warning and TODO counts on each PR | every PR |

## Checks worth a closer look

- **Accessibility on every page.** One test per prerendered route runs axe-core against WCAG 2.2 A and AA, and also asserts exactly one visible `h1`, the SEO title and no horizontal scrolling.
- **Visual regression.** Screenshot comparison with a 1% pixel tolerance. Baselines are generated inside the official Playwright Docker image pinned to the same version as `package-lock.json`, so results do not depend on the CI host's fonts or OS.
- **Performance budgets.** On an emulated slow phone (CPU and network throttling over CDP), key pages must stay under an LCP limit and a CLS of 0.05. The test was written after a real regression: an entry animation pushed LCP from 1.5 s to 8 s.
- **A race-condition guard.** A React hydration error (#421) appeared in about 25% of page loads under slow conditions. The test delays route bundles and slows the CPU 4×, then loads the page 24 times: enough to catch a regression with about 99.9% probability, while the fixed version shows 0 errors.
- **Feature-flag builds.** A second CI job builds the site with the redesign flags switched on and runs the same suite, so what is about to go to production is tested before the flags change.

## Pipeline

1. `npm run lint` → `npm test` → `npm run build` → `npm run test:e2e`, in that order, failing fast.
2. On failure, Playwright's expected, actual and diff images are kept as artifacts.
3. Because hosted runners were unreliable for this project, the suite also runs on my own Linux server through [AI Test Runner 2.0](https://github.com/TomekWojciechowski/AI-TESTING-INFRASTRUCTURE), which tests the exact pull-request commit in rootless Podman and posts the result back to the PR.

## What I would point out in an interview

- Each non-obvious test has a comment explaining which bug it guards against and why its threshold is set where it is.
- Engineering decisions (for example running tests locally instead of relying on hosted CI) are recorded with the evidence behind them.
