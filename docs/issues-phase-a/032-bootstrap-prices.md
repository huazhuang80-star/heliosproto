# 032 — Bootstrap prices service with Reflector adapter

**Pillar:** `backend/`
**Difficulty:** intermediate
**Estimated effort:** 6h
**Reward tier:** medium

## Goal

Stand up `backend/prices` as a service that polls the Reflector oracle on Soroban for normalized USD prices and exposes them via `GET /price/{symbol}`.

## Context

Portfolio valuation, send-form fiat conversion, and the dashboard all need USD prices. Reflector is Stellar's primary on-chain oracle. This issue lands the first adapter; Chainlink / Band adapters are later.

Depends on issue **030** (Settings).

## Scope

**Touch:**
- `backend/prices/pyproject.toml` (new)
- `backend/prices/main.py` (new — FastAPI)
- `backend/prices/config.py` (new — copy 030's pattern, add `REFLECTOR_ORACLE_ADDRESS`)
- `backend/prices/reflector.py` (new — adapter)
- `backend/prices/tests/test_reflector.py` (new)
- `backend/prices/tests/test_endpoint.py` (new)
- `backend/prices/Dockerfile` (new)
- `backend/prices/.dockerignore` (new)
- `backend/docker-compose.yml` — add `prices` service

**Do not touch:**
- `backend/api/` or `backend/indexer/`

## Acceptance criteria

- [ ] `ReflectorAdapter.get_price(symbol) -> PriceQuote { price_usd: Decimal, timestamp: datetime, source: "reflector" }`
- [ ] Calls Reflector's `lastprice` contract function via Soroban RPC; decodes the returned `i128` to a `Decimal` USD value
- [ ] In-memory TTL cache (30s by default, configurable) — repeat calls within TTL don't hit RPC
- [ ] `GET /price/{symbol}` returns the cached `PriceQuote` as JSON
- [ ] Returns 404 with a clear error if the symbol isn't supported by Reflector
- [ ] Returns 503 with `Retry-After` if Reflector is unreachable
- [ ] `ruff`, `ruff format --check`, `mypy --strict`, `pytest` clean

## Tests required

- Adapter returns expected `PriceQuote` for a mocked RPC response
- Cache: second call within TTL doesn't trigger RPC (use a counter)
- 404 returned for unsupported symbol
- 503 returned when RPC times out

## References

- [Reflector docs](https://reflector.network/docs)
- Depends on: [030 — Pydantic Settings](030-pydantic-settings.md)
