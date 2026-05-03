# Weather Pipeline Automation Plan

## Goal
Automate `weather.py` to run on a schedule using GitHub Actions, committing updated forecast data back to the repo.

## Design Decisions

**API key handling:** Refactored `weather.py` to read the key via `os.getenv("WEATHER_API_KEY")` instead of hardcoding it. The key is stored as a GitHub Actions repository secret.

**Triggers:** Both `workflow_dispatch` (manual) and `schedule` (cron). Manual trigger allows testing before trusting the schedule.

**Run cadence:** Daily at 8am UTC (`0 8 * * *`). Weather forecasts update daily, so running more frequently would waste API credits without adding data value.

**Commit-back:** The workflow commits the updated `weather_data.csv` to `main` using the `github-actions[bot]` identity. Requires `permissions: contents: write` in the workflow.

**Failure handling:** If the API call fails, the script will error and the workflow step will fail — GitHub will mark the run red and notify via email. No silent failures.

## Secrets Required
- `WEATHER_API_KEY` — WeatherAPI key, set in repo Settings → Secrets and variables → Actions
