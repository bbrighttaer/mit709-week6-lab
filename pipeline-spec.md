# StudySpace Pipeline Specification

The pipeline protects the StudySpace booking rules: US-03 / PS-03 (booking length), US-04 / PS-04 (two future bookings) and US-05 / PS-05 (cancellation cut-off). Each rule below has an ID. Use the IDs in step names, commit messages and your lab log.

## Part 1: Build, test and deliver (Task 1)

| ID | Rule |
|---|---|
| CI-01 | Run on every push to any branch, and on every pull request to `main`. |
| CI-02 | Run on `ubuntu-latest` with Python 3.12, and install dependencies with `pip install -r requirements.txt`. |
| CI-03 | Run `pytest test_booking_rules.py test_booking_store.py --cov=booking_rules --cov=booking_store --cov-branch --cov-report=term-missing --cov-fail-under=90`. The run must fail if coverage is below 90%. |
| CI-04 | Run `python mutants.py test_booking_rules.py --min-score 90`. The run must fail if the mutation score is below 90%. |
| CI-05 | Deliver: only on a push to `main`, and only after every check has passed, upload `booking_rules.py` and `booking_store.py` as a build artefact named `studyspace-release`. |
| CI-06 | The deliver job must use an environment named `production`, so that a person approves it before it runs. |

## Part 2: Quality and security checks (Task 2)

| ID | Rule |
|---|---|
| CI-07 | Lint all Python files with `ruff check .` |
| CI-08 | Scan the application code for security issues with `bandit booking_rules.py booking_store.py` |
| CI-09 | Audit dependencies for known vulnerabilities with `pip-audit -r requirements.txt` |

## Part 3: Hardening (Task 3)

| ID | Rule |
|---|---|
| CI-10 | Audit the workflow itself with `zizmor --offline .github/workflows/`, and fix every finding. |
| CI-11 | The workflow's token gets read-only access (`contents: read`) unless a job needs more. |
| CI-12 | Every action is pinned to a full commit SHA, with the version in a comment. |
| CI-13 | No check may be skipped or allowed to fail silently (no `continue-on-error`, no `|| true`). |
