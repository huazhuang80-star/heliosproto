# 033 — Implement SEP-10 web auth challenge + verify endpoints

**Pillar:** `backend/`
**Difficulty:** intermediate
**Estimated effort:** 8h
**Reward tier:** medium

## Goal

On `backend/api`, implement SEP-10 web authentication: a challenge endpoint that returns a Stellar transaction, and a verify endpoint that validates the signed challenge and issues a JWT.

## Context

SEP-10 is the standard auth flow used by every Stellar anchor and most wallet integrations. The Helios wallet uses it to authenticate against the backend (e.g., to subscribe to notifications, to upload encrypted guardian metadata). Indexer and notifications services will all gate on a SEP-10-issued JWT.

Depends on issue **030** (Settings).

## Scope

**Touch:**
- `backend/api/auth/__init__.py` (new)
- `backend/api/auth/sep10.py` (new)
- `backend/api/auth/jwt.py` (new)
- `backend/api/main.py` (mount the auth router)
- `backend/api/tests/test_sep10.py` (new)
- `backend/api/pyproject.toml` (add `pyjwt[crypto]`, `stellar-sdk`)

**Do not touch:**
- Other backend services

## Acceptance criteria

- [ ] `GET /auth?account=G…` returns `{ transaction, network_passphrase }` per SEP-10 v3
- [ ] Challenge transaction has correct `manageData` op, time bounds, source = server signing key
- [ ] `POST /auth { transaction }` verifies the signed challenge per SEP-10 and returns `{ token: <JWT> }`
- [ ] JWT signed with HS256 (key from settings), claims include `sub` (the authed account), `iat`, `exp` (15 min), `iss` (configurable)
- [ ] Verify rejects: wrong server signature, expired time bounds, invalid client signature, replay (same tx twice)
- [ ] All endpoints validate input with Pydantic; 400 on malformed account, 401 on bad signature
- [ ] `ruff`, `ruff format --check`, `mypy --strict`, `pytest` clean

## Tests required

- Challenge endpoint returns well-formed transaction
- Verify accepts a correctly signed challenge (use stellar-sdk to construct + sign in the test)
- Verify rejects: wrong server sig, expired tx, missing client sig, replay
- JWT claims are correct

## References

- [SEP-10 Stellar Web Authentication](https://github.com/stellar/stellar-protocol/blob/master/ecosystem/sep-0010.md)
- [stellar-sdk `Sep10Helper`](https://stellar-sdk.readthedocs.io/en/stable/api.html) (Python)
- Depends on: [030 — Pydantic Settings](030-pydantic-settings.md)
