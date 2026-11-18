# token-radar

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/lpy214/token-radar/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/lpy214/token-radar/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

Token Radar ranks every genuine pre-token protocol by what its launch would be worth, and shows how to farm each one. It reads a sealed DefiLlama snapshot, scores 198 protocols with the radar-2 model, and writes a static dashboard a reader can sort, filter and open protocol by protocol.

## The model (radar-2)

Every row is scored by one function of the snapshot's own figures, never by a hand-kept list of names:

```
implied = round(tvl_usd x category multiple
  x (1 + clamp(change_7d, -60, 120) / 100 x 0.6)
  x maturity x chain reach)
```

- **Category multiple** — a FDV/TVL multiple by category: Dexs 0.45, Lending 0.42, Derivatives 0.40, Risk Curators 0.36, RWA 0.32, Yield 0.30, everything else 0.25.
- **Maturity** — `0.55 + min(age_days, 1095) / 1095 x 0.45`; age is days since first listed, unknown age uses 365 days.
- **Chain reach** — 3% per extra chain, up to four.

A protocol is rankable only when the snapshot records no token for it at all and it is not a wrapped, staked or bridged product of a token that already trades. That gives 40 watchlist protocols plus 158 early ones below the $50M floor. Live-token protocols are set aside with their market caps shown, never mixed into the ranking.

## The dashboard

- A board of 198 protocols, sortable by rank, TVL, 7-day growth, age and implied value.
- Filters by search, category, chain and scope (watchlist / early), plus a live-token tab.
- A detail view for every protocol with the model's full breakdown and a 7-step farm plan, each step with its cost and time.
- A model sandbox: move the growth-weight and base-scale sliders and the whole board recomputes from the same figures.
- No network: the page is static HTML, CSS and JavaScript reading `data.js` beside it.

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input <snapshot.json>
```

`main.ts` (Node 22, standard library only) reads the snapshot, writes `board.json` and `data.js` to the output directory, copies `dashboard/` and `screenshots/` there, and writes `dossier.json` with the vendored `vendor/makefun-board.ts`.

## Layout

- `main.ts` — the entry program.
- `lib/model.ts` — the radar-2 model, farm plans and snapshot parsing.
- `vendor/makefun-board.ts` — make.fun's dossier library, copied byte for byte from the kit.
- `dashboard/` — the static page: `index.html`, `app.css`, `app.js`.
- `screenshots/` — the board, a protocol detail view and a filtered view, rendered in a headless browser from this dashboard.

---

<sub>Entered on [make.fun](https://make.fun/?ref=g0cnq9c3&utm_source=github&utm_medium=readme&utm_campaign=token-radar-01) · AI agents build software for cash rewards</sub>
