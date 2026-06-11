# 031 — Bootstrap indexer service with HTTP polling skeleton

**Pillar:** `backend/`
**Difficulty:** intermediate
**Estimated effort:** 6h
**Reward tier:** medium

## Goal

Stand up `backend/indexer` as a real service: own `pyproject.toml`, an async polling loop hitting Soroban RPC's `getLatestLedger`, structured logging, and graceful shutdown. No DB writes yet — those land in a follow-up.

## Context

Every wallet feature beyond "submit a tx" needs indexed state — balances, history, plugin events. The indexer is the data plane. Landing the polling skeleton now means parallel issues can plug in decoders and writers without restructuring the runtime.

Depends on issue **030** (Settings pattern to copy).

## Scope

**Touch:**
- `backend/indexer/pyproject.toml` (new)
- `backend/indexer/main.py` (new)
- `backend/indexer/config.py` (new — copy the `Settings` pattern from 030)
- `backend/indexer/tests/test_polling.py` (new)
- `backend/indexer/.dockerignore` (new)
- `backend/indexer/Dockerfile` (new)
- `backend/docker-compose.yml` — add `indexer` service

**Do not touch:**
- `backend/api/`
- Database — no DB writes in this issue

## Acceptance criteria

- [ ] Service starts via `python -m backend.indexer.main` (or `uvicorn`-style if HTTP needed for healthcheck)
- [ ] Async loop polls Soroban RPC's `getLatestLedger` at a configurable interval (default 5s)
- [ ] Each tick logs `{ledger_seq, ledger_hash, latency_ms}` via `structlog` or `logging` with JSON formatter
- [ ] On SIGTERM / SIGINT, drains the current poll and exits cleanly
- [ ] Resilient to transient errors: 5xx / network errors logged and retried with backoff, do not crash the loop
- [ ] Exposes a `/health` endpoint (FastAPI or aiohttp) for docker-compose healthcheck
- [ ] `ruff`, `ruff format --check`, `mypy --strict`, `pytest` clean

## Tests required

- Polling loop runs N iterations against a mocked RPC client
- Mocked 503 response triggers retry, not crash
- Healthcheck returns ok while loop is running

## References

- [Soroban RPC `getLatestLedger`](https://developers.stellar.org/docs/data/rpc/api-reference/methods/getLatestLedger)
- [`httpx` async client](https://www.python-httpx.org/async/)
- Depends on: [030 — Pydantic Settings](030-pydantic-settings.md)
