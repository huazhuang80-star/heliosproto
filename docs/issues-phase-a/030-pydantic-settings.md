# 030 — Add Pydantic Settings with env loading and DI

**Pillar:** `backend/`
**Difficulty:** good-first-issue
**Estimated effort:** 3–4h
**Reward tier:** small

## Goal

Replace hardcoded config (`STELLAR_RPC_URL`, `DATABASE_URL`, `REDIS_URL`) with a single `Settings` class loaded from environment / `.env`, exposed via FastAPI's `Depends`.

## Context

Every backend service will need this pattern. Landing it once in `backend/api` gives the indexer (031), prices (032), and SEP-10 auth (033) issues a clean template to copy.

## Scope

**Touch:**
- `backend/api/config.py` (new)
- `backend/api/main.py` (use `Depends(get_settings)` in at least one endpoint)
- `backend/api/tests/test_config.py` (new)
- `backend/api/pyproject.toml` — `pydantic-settings` is already in deps; just verify

**Do not touch:**
- Other backend services

## Acceptance criteria

- [ ] `Settings(BaseSettings)` with fields: `stellar_rpc_url: str`, `stellar_network: Literal["testnet","futurenet","mainnet"]`, `database_url: str`, `redis_url: str`, `sentry_dsn: str | None`
- [ ] `model_config = SettingsConfigDict(env_file=".env", extra="ignore")`
- [ ] `get_settings()` is `@lru_cache`-decorated so it's instantiated once per process
- [ ] `/health` endpoint extended to include `network` in its response (using `Depends(get_settings)`)
- [ ] `HealthResponse` updated; test updated to assert the new field
- [ ] `ruff check`, `ruff format --check`, `mypy --strict main.py config.py`, `pytest` all green

## Tests required

- Loading with all required env vars set returns a valid `Settings`
- Missing `STELLAR_RPC_URL` raises `ValidationError`
- `lru_cache` returns the same instance across calls
- `/health` returns the configured network

## References

- [pydantic-settings docs](https://docs.pydantic.dev/latest/concepts/pydantic_settings/)
- [FastAPI dependencies](https://fastapi.tiangolo.com/tutorial/dependencies/)
