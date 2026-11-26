---
trigger: model_decision
description: "Apply when adding features, providers, backtesting, screeners, or tests in Trading-Lab."
---

# Trading-Lab SOLID Architecture & Testing Standards

- **Target Runtime:** Python 3.12 managed with Poetry (`poetry env use /usr/bin/python3.12`).
- **SOLID Dependency Inversion:** All external integrations and pipeline stages must implement the ABC contracts in `src/trading_lab/interfaces/contracts.py` (`MarketDataProvider`, `NewsProvider`, `QuantitativeAnalyzer`, `LLMThesisEngine`, `RiskGate`, `BrokerExecutor`, `TradeLedgerRepository`, `ReflectionService`). Avoid `hasattr()` duck-typing across service boundaries.
- **Test Hygiene:** Put shared fixtures in `tests/conftest.py`. Every guardrail, math edge case, JSON parsing fallback, and persistence path must have deterministic offline unit tests (never hit live external APIs in unit tests).
- **Dashboard Markers:** Preserve `<title>Trading Lab — Autonomous Paper-Trading Dashboard</title>`, `data-trading-lab-health="ok"`, and `/health` JSON fields (`db_ok`, `ollama_ok`, `scheduler_running`, `paper_only`) checked by `Homelab-Ansible`'s post-deploy verification gate.
