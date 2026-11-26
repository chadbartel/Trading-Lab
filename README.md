# Trading-Lab

Autonomous, paper-only AI trading lab built with **Python 3.12**, **Poetry**, **FastAPI**, **TimescaleDB**, **APScheduler**, and **Ollama**.

## Architecture (SOLID OOP)
- `src/trading_lab/interfaces/contracts.py` — Abstract base classes for all pipeline stages
- `src/trading_lab/providers/` — `AlpacaMarketDataProvider` (`alpaca-py >= 0.45.0`), `YahooChartMarketDataProvider`, RSS/Alpaca news providers, deterministic offline fallback
- `src/trading_lab/analytics/quantitative.py` — Pure deterministic math (RSI-14, ATR-14, SMA-20/50, EMA-12/26, 20d realized volatility)
- `src/trading_lab/llm/ollama_engine.py` — Structured JSON thesis & reflection engine (`think=false`, `format=json`, safe `HOLD` fallback)
- `src/trading_lab/risk/gate.py` — Hard-coded deterministic guardrails (paper-only assertion, ticker allowlist, 5% max position cap, mandatory stop-loss, 3% daily drawdown kill-switch)
- `src/trading_lab/execution/broker.py` — Alpaca Paper Trading broker with automatic fallback to Internal Virtual Ledger mode
- `src/trading_lab/persistence/repository.py` — SQLAlchemy 2.0 + TimescaleDB PG16 hypertables (`market_bars`, `portfolio_snapshots`) with SQLite support for unit tests
- `src/trading_lab/services/` — `TradingPipelineOrchestrator` & `WeeklyReflectionService`
- `src/trading_lab/scheduler/runner.py` — Embedded `AsyncIOScheduler` (weekday market-hours cycle + reflection job)
- `src/trading_lab/web/` — FastAPI dark-mode `SPY` benchmark dashboard & JSON control/health APIs

## Local Development (Python 3.12)
```bash
poetry env use /usr/bin/python3.12
poetry install
timeout 60 poetry run pytest -v
timeout 60 poetry run ruff check src tests
```

## Homelab Deployment
Production deployment on `monolith` (`192.168.1.17`) is managed declaratively by `../Homelab-Ansible` (`roles/trading_lab`, `--tags trading_lab`) with persistent data under `/mnt/ollama_storage/trading_lab/` and Let's Encrypt HTTPS at `https://trading.chadbartel.duckdns.org`.
