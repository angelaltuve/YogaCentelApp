# AGENTS.md — High-Signal OpenCode Guidance

## Entry and Workflow
- **Main entry:** Run `python main.py` from an environment where all dependencies (Poetry-managed) are installed.
- **First run auto-bootstraps:** On first launch, a default admin user (admin@yogacenter.com / admin123) and a default center are created if absent. No manual DB seeding required.
- **App requires GUI:** This is a desktop PyQt6 app—headless runs will fail unless using a Qt event loop/mocking.

## Environment and Dependencies
- **Python 3.8+ required.** See `pyproject.toml` for constraints. Poetry is the package manager. Activate environment via `poetry shell`.
- **Install:** `poetry install` (from repo root)
- **Run:** `python main.py` (after `poetry shell`)
- **Dependencies**: 
  - Core: `pyqt6`, `sqlmodel`, `bcrypt`
  - Dev/test: `pytest`, `pytest-mock`, `pytest-qt`, `pytest-cov`, `pytest-html` (see `[dependency-groups.dev]` in `pyproject.toml`)

## Database and Persistence
- **SQLite only.** DB file is at `data/database.db`, created at runtime if missing.
- **No environment variables required by default** (db path is auto-handled in code).
- **Schema changes:** Run the app or tests; tables are always auto-created if missing.

## Testing
- **Testing uses transaction and engine patching.** Tests require patched in-memory DB with auto monkeypatching via `tests/conftest.py` fixtures.
- **Test a single file:** `poetry run pytest tests/test_main_window.py`
- **Run all tests:** `poetry run pytest`
- **UI test support:** Use `qtbot` fixture from `pytest-qt` to handle PyQt widgets in tests.
- **Database and session management in tests is handled by fixtures**—do NOT connect to real DB in tests.
  
## Coverage and Reports
- **Generate coverage and HTML report:**
  - `poetry run pytest --cov --cov-report=html --html=reports/report.html`
- **Coverage html output:** `reports/coverage/index.html`

## Style and UX
- **UI styling is loaded from `styles/styles.qss`** at launch. Error is printed if missing, but app continues.

## Notable Conventions & Gotchas
- **Tab names in UI use emoji prefixes.**
- **Admin/Center required:** App prints warning if no center exists on startup, but continues.

---
- No `opencode.json`, custom cursor rules, or repo-local agent standards exist as of writing.
- For major logic boundaries, see `/ui` (widgets, dialogs) and `/database/db.py` (all persistence logic). Entry is `main.py`.
- Ignore generic Python, PyQt, or Poetry advice—only what's above is repo-specific.
