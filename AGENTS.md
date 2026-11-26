# Trading-Lab Repository Rules

## Non-Negotiable Safety Guardrails
- **Paper-Trading Only:** Never disable `PAPER_ONLY=true` or target live brokerage endpoints (`api.alpaca.markets`). Alpaca integration MUST strictly assert HTTPS `paper-api.alpaca.markets` (`paper=True`) or fall back cleanly to the internal Virtual Ledger mode.
- **Deterministic RiskGate Isolation:** Never allow LLM output to bypass or weaken `DeterministicRiskGate` (ticker allowlist, 5% max position cap, mandatory stop-loss, daily drawdown kill-switch, `math.isfinite()` checks). Keep `QuantitativeAnalyzer` 100% deterministic with zero LLM calls.
- **Zero Secrets in Repo:** NEVER read, print, edit, or commit `.env`, API keys, passwords, or `.vault_pass`. All credentials enter exclusively via environment variables (managed by `Homelab-Ansible` vault in production).
- **WSL2 Constraints:** Never invoke Windows `.exe` binaries (e.g., `docker.exe`); wrap local test/lint commands in `timeout 60`.

## Deployment Boundary (`monolith` — `192.168.1.17`)
- Runtime deployment, NVMe storage (`/mnt/ollama_storage/trading_lab/`), CPU-only Compose V2 limits, Pi-hole DNS, and Let's Encrypt wildcard TLS (`https://trading.chadbartel.duckdns.org`) are codified in `../Homelab-Ansible` (`roles/trading_lab`). Do not make ad-hoc production edits over SSH.
- Trading Lab containers remain CPU-only (Ryzen 9 5900XT); LLM inference delegates to the existing `ollama` service (`http://ollama:11434`, RTX 2070 SUPER 8 GB VRAM) over `homelab-bridge`.

## Progressive Disclosure Pointers
- **Risk & Paper-Trading Invariants:** See `.agents/rules/risk-and-paper-safety.md` when editing `src/trading_lab/**/*.py`.
- **SOLID Architecture & Testing Conventions:** See `.agents/rules/architecture-and-testing.md`.
- **Test & Lint Runbook:** Activate `.agents/skills/test-and-lint/SKILL.md` before committing changes.
- **Shared Architecture Wiki:** Read `/home/thatsmidnight/projects/My-LLM-Wiki/Wiki/index.md` (and `Wiki/entities/Trading-Lab.md`, `Wiki/entities/Monolith.md`).
