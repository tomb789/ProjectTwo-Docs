# Bowling Metrics (Authoritative)
Version: v1.0 • Last updated: 2025-10-25

## Purpose
Canonical definitions and column order for bowling metrics (season and career aggregations).

**Source views**  
`main.v_bowling_totals_season` (career equivalent view planned)

**Input basis**  
`staging.v_deliveries_enriched` using legality flag `is_legal_delivery_bowler`

**Global exclusions**  
Super overs (innings 3 and 4) are excluded from all metrics.

---

## Column Order (Final)
year, player_name, matches, innings, balls, runs, wickets,
bbi, average, economy, er_per_ball, strike_rate,
fours, sixes, dots, B_percent, D_percent,
three_fers, five_fers


---

## Metric Definitions

- **matches**  
  Distinct matches where the player bowled at least one legal ball (innings_number in {1,2}).

- **innings**  
  Bowling innings where the player bowled at least one legal ball.

- **balls**  
  Count of deliveries with `is_legal_delivery_bowler = TRUE`.

- **runs**  
  Total runs conceded by the bowler.

- **wickets**  
  Dismissals credited to the bowler  
  (exclude run-outs not credited to the bowler).

- **bbi (Best Bowling)**  
  Best wickets-runs figure in a single match (e.g., 5/22).  
  Text `W/R` recommended for display.

- **average**  
  `runs / wickets` if `wickets > 0`, else `NULL`.

- **economy**  
  `runs / (balls / 6)` using only legal balls.

- **er_per_ball**  
  `runs / balls`.  
  If `balls = 0`: `NULL`.

- **strike_rate**  
  `balls / wickets` if `wickets > 0`, else `NULL`.

- **fours**  
  Boundary fours conceded.

- **sixes**  
  Boundary sixes conceded.

- **dots**  
  Legal deliveries conceded for 0 runs.

- **B_percent (Boundary % conceded)**  
  `boundary_runs_conceded / runs`  
  where `boundary_runs_conceded = 4*fours + 6*sixes`.  
  If `runs = 0`: `NULL`.

- **D_percent (Dot %)**  
  `dots / balls`.  
  If `balls = 0`: `NULL`.

- **three_fers**  
  Innings with exactly 3 wickets.

- **five_fers**  
  Innings with at least 5 wickets.

---

## Counting & Legality Rules
- Use **`is_legal_delivery_bowler`** for ball counts.
- Exclude super overs (innings 3–4).
- Economy only counts legal balls (balls / 6).

---

## Reconciliation Expectations
For every context grouping:  
The sum of **balls**, **runs**, and **wickets** must match the player-season totals exactly.

---

## Status
Authoritative and enforced.
