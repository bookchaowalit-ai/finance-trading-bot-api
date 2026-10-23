# finance-trading-bot-api (formerly bookchaowalit-trading-backend)

**Owner:** `bookchaowalit-ai`  
**Default branch:** `main`  
**Role:** Backend/services supporting trading automation experiments (bots, logs, migration notes).

## What this is

A nested backend checkout used with the broader trading portfolio (see sibling
`booktrading` product under `bookchaowalit-ai/book-products/`). This tree holds
operational pieces such as `trading-bots/` and migration notes — it is **not**
the full interview UI/product case study.

## Quick layout

| Path | Role |
|---|---|
| `trading-bots/` | Bot code / runtime experiments |
| `NEON_MIGRATION_COMPLETE.md` | DB migration note |
| `logs/` | Local logs, gitignored (untracked since 5cbf298) |
| `.env.example` under bots (if present) | Template only |

## Auth / secrets

- Use `.env.example` / templates only in Git.
- Never commit live exchange keys, webhook URLs, or DB credentials.
- Prefer read-only / paper modes unless a written kill-switch policy exists
  (see `booktrading` PRODUCT for observe-first philosophy).

## Run (high level)

```bash
# Inspect first — stack varies by bot package
ls trading-bots
# Follow per-bot docs/requirements if present
```

## Tests / quality

Offline tests (no database, no exchange calls) live in `trading-bots/tests`
and run in CI (`.github/workflows/ci.yml`):

```bash
cd trading-bots
python -m pip install -r requirements-test.txt
python -m ruff check . --select E9,F63,F7,F82
python -m pytest -q
```

## Trading mode

Orders reach a real account only when `TRADING_MODE=live` is set exactly.
Any other value (unset, `paper`, typos) wraps every exchange connector in
`src/exchanges/paper_exchange.py`: tickers stay real, while balances and
fills are simulated in memory.

## Related

- Interview flagship: `bookchaowalit-ai/finance-booktrading` (formerly `booktrading`)
- Org transfer to `bookchaowalit-backend` is **deferred** (Book Dev BD-034)
- Catalog: Solo Empire `BOOK-DEV-BACKLOG-BD.md` BD-013

## Disposition

Recommendation (2026-10-23, Big Loop review): **archive**; the owner decides and archives.

- Superseded: `finance-booktrading/strategy/app` already has grid (paper and real with
  preflight), DCA, trend, arbitrage, a risk manager, paper execution and Bitkub/Binance TH
  sources. This repo is the older, smaller version of the same idea.
- Not deployed anywhere, no live URL, 0 stars/forks; last code change 2026-09-30, last commit
  2026-10-12 (licence).
- Possibly unique: `momentum_strategy.py`, `thai_stock_strategy.py` and the InnovestX connector.
  If wanted, port them into finance-booktrading (that repo's owner session) before archiving.
- Until archived, CI stays on the shared workflows; it defaults to paper mode (see above).
- Security history (roadmap trd-g1): a Neon password was committed before redaction in
  NEON_MIGRATION_COMPLETE.md; rotation must be confirmed by the owner. Archiving does not
  remove it from history.
