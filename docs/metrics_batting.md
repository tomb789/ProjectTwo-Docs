# Batting Metrics (Authoritative)
Version: v1.0 • Last updated: 2025-10-25

## Purpose
Canonical definitions and column order for batting metrics (season and career aggregations).

**Source views**  
`main.v_batting_totals_season` (career equivalent view planned)

**Input basis**  
`staging.v_deliveries_enriched` using legality flag `is_ball_faced_batter`

**Global exclusions**  
Super overs (innings 3 and 4) are excluded from all metrics.

---

## Column Order (Final)
year, player_name, matches, innings, runs, balls_faced,
outs, not_outs, average, strike_rate,
HS, fours, sixes, dots, B_percent, D_percent,
fifties, hundreds

---

## Metric Definitions

- **matches**  
  Distinct matches where the player batted at least once (innings_number in {1,2}).

- **innings**  
  Batting innings where `is_ball_faced_batter = TRUE` or a dismissal is recorded.

- **runs**  
  Sum of `runs_batter_credit` across legal batting deliveries.

- **balls_faced**  
  Count of deliveries with `is_ball_faced_batter = TRUE`.  
  • Wides do not count  
  • No-balls count only if a legal ball was delivered

- **outs**  
  Dismissals where `player_out_name = player_name`.

- **not_outs**  
  `innings - outs`.

- **average**  
  `runs / outs` if `outs > 0` else `NULL`.

- **strike_rate**  
  `100 * runs / balls_faced`.

- **HS (High Score)**  
  Maximum single-innings runs.  
  Stored numeric does not include `*` marker.

- **fours**  
  Boundary fours hit by the batter.

- **sixes**  
  Boundary sixes hit by the batter.

- **dots**  
  Legal balls faced with `runs_batter_credit = 0`.

- **B_percent (Boundary %)**  
  `boundary_runs / runs`  
  where `boundary_runs = 4*fours + 6*sixes`.  
  If `runs = 0`: return `NULL`.

- **D_percent (Dot %)**  
  `dots / balls_faced`.  
  If `balls_faced = 0`: `NULL`.

- **fifties**  
  Innings with 50–99 runs.

- **hundreds**  
  Innings with 100+ runs.

---

## Counting & Legality Rules
- Use **`is_ball_faced_batter`** for ball counts.
- Exclude super overs (innings 3–4).
- Wides never count as balls faced.

---

## Reconciliation Expectations
For every context grouping:  
The sum of **runs**, **balls_faced**, and **outs** must match the player-season totals exactly.

---

## Status
Authoritative and enforced.
