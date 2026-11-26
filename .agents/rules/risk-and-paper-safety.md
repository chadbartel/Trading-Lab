---
trigger: glob
globs: "src/trading_lab/**/*.py"
---

# Risk Gate & Paper-Trading Safety Invariants

1. **Strict Paper-Only Enforcement:**
   - `TradingLabSettings.paper_only` must default to `True` and be enforced in both `DeterministicRiskGate` and `AlpacaPaperBrokerExecutor`.
   - Validate `alpaca_paper_base_url` with `urllib.parse.urlparse`: require `scheme == "https"` and `hostname == "paper-api.alpaca.markets"`.
2. **Hard-Coded RiskGuardrails (`risk/gate.py`):**
   - Reject non-finite (`NaN`/`Inf`) numeric inputs via `math.isfinite()`.
   - Enforce ticker allowlist, single-position + cumulative 5.0% equity cap (`max_position_pct`), mandatory bounded stop-loss on every BUY, and daily drawdown kill-switch (`daily_drawdown_limit_pct`).
3. **LLM Fault Tolerance (`llm/ollama_engine.py`):**
   - Always pass `format: "json"` and `think: false` (for Qwen3 compatibility) to Ollama `/api/chat`.
   - Malformed JSON, network failures, or timeouts MUST degrade safely to a `HOLD` thesis (`fallback_used=True`) rather than crashing the scheduler or placing orders.
4. **TimescaleDB Idempotency (`persistence/repository.py`):**
   - Keep schema DDL idempotent (`CREATE TABLE IF NOT EXISTS`, `CREATE EXTENSION IF NOT EXISTS timescaledb`, `create_hypertable(..., if_not_exists => TRUE)`) while retaining full SQLite compatibility for unit tests.
