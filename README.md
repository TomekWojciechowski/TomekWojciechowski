# Tomasz Wojciechowski

**Test Automation Engineer · Python, Java and SQL · API and data testing**

I build automated checks for APIs and data pipelines. At QESTIT, I tested financial data mappings against business rules in Python. At Capgemini, I automated checks on API responses in Java. The projects below show how I approach both kinds of work.

| Project | What to look at |
| --- | --- |
| [API testing in Python](https://github.com/TomekWojciechowski/api-testing-python) | pytest and httpx tests for a payments API running in Docker: strict response contracts, data-driven negative tests, idempotent retries and SQL checks that every transfer writes a matching debit and credit. The README shows which injected bugs the suite catches. |
| [UI and API test automation in Java](https://github.com/TomekWojciechowski/qa-automation-framework) | Selenium page objects for a demo shop's checkout and REST Assured tests for a booking API. TestNG runs in parallel; GitHub Actions publishes an [Allure report](https://tomekwojciechowski.github.io/qa-automation-framework/). |
| [AI Test Runner 2.0](https://github.com/TomekWojciechowski/AI-TESTING-INFRASTRUCTURE) | A self-hosted CI runner in Python that checks a pull request's authorisation and exact SHA, runs ESLint, unit, build and Playwright stages in rootless Podman and posts the result to the PR. 38 unit tests with mocked GitHub, Git and Podman. |
| [Polska 2038: testing case study](case-studies/polska-2038-testing.md) | How I test a 40-page React and FastAPI site: Playwright on desktop and phone (456 passing in the last full run), axe-core accessibility checks on every page, visual regression, performance budgets and pytest API tests. |
| [Financial transaction reconciliation](https://github.com/TomekWojciechowski/python-data-validation-demo) | A Python validator compares a CSV with a SQLite ledger and reports a one-cent discrepancy by transaction ID, covered by pytest. |

**Tools:** Python (pytest, httpx, pandas), Java (TestNG, REST Assured, Selenium), SQL, Robot Framework, Playwright, Docker, Podman, systemd, GitHub Actions, GitLab CI, Allure, Postman, Jira

**Languages:** Polish (native), German, English · Based near Görlitz/Zgorzelec, open to roles in Poland and Germany

[Connect with me on LinkedIn](https://www.linkedin.com/in/tomasz-wojciechowski-68a39a268/)
