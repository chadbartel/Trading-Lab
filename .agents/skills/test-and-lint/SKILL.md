---
name: test-and-lint
description: >-
  Run the mandatory local Python 3.12 Poetry validation gate for Trading-Lab:
  pytest unit test suite, ruff linter, and syntax verification.
---

# Trading-Lab Test & Lint Gate

Run all checks from `/home/thatsmidnight/projects/Trading-Lab` before committing:

```bash
timeout 60 poetry run pytest -v
timeout 60 poetry run ruff check src tests
```

## Pass Criteria
- `pytest`: 100% of tests pass with zero failures or errors.
- `ruff check`: 0 lint errors across `src/` and `tests/`.
- No secret files (`.env`, API keys) or ephemeral agent logs staged in `git status`.
