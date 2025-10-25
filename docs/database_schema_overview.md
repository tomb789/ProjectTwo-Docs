File: database_schema_overview.md

Purpose: High-level schema, DB layout, keys, join logic, and engine-specific notes.

1) Databases and schemas

staging.db (DuckDB): Holds raw-normalized and enriched staging views from Cricsheet, notably staging.v_deliveries_enriched and staging.stg_matches.

metrics.db (DuckDB): Holds analytical views used by dashboards:

Totals: main.v_batting_totals_season, main.v_bowling_totals_season

Contexts: main.v_batting_ctx_*, main.v_bowling_ctx_* (by venue, by over, by innings, by position, by result, vs hand/style, etc.)

Global rule: Super overs excluded in all analytical views.

2) Key fields and legality flags

Batting legality: is_ball_faced_batter

Bowling legality: is_legal_delivery_bowlerApply these consistently for ball counts, dot-ball logic, strike rate and economy calculations.

3) Primary keys and foreign keys (logical)

DuckDB does not enforce FK constraints natively; treat these as logical constraints in views and validation scripts.

Primary Keys (logical):
  deliveries: (match_id, innings_number, ball_id)
  matches:   (match_id)

Foreign Keys (logical):
  deliveries.match_id → matches.match_id
  deliveries.batter_name ↔ players.name   (if/when a players dimension is added)
  deliveries.bowler_name ↔ players.name   (ditto)

4) Core joins

Totals join deliveries to matches on match_id to access date (for year) and venue (ground, city) and match result.

Contexts extend totals grouping by the relevant context dimension (e.g., over, venue, batter_hand, bowling_style, position, match_innings).

5) Engine notes (DuckDB)

Views are lightweight; prefer CREATE OR REPLACE VIEW for iterative development.

Booleans are first-class; keep legality flags as BOOLEAN.

No implicit time zone handling for naive dates; store match dates as DATE.

Aggregations over HUGEINT/UBIG types are fine; cast where needed for division to avoid integer division.

Validation lives in Python scripts executing SQL against DuckDB (your scripts/validation/*.py).

6) ASCII relationship sketch

[staging.stg_matches] (match_id, date, ground, city, result, ...)
          │
          │  (match_id)
          ▼
[staging.v_deliveries_enriched]  (match_id, innings_number, ball_id, batter_name, bowler_name,
   is_ball_faced_batter, is_legal_delivery_bowler, runs_batter_credit, runs_total_credit,
   wicket_type, player_out_name, over, bowling_style, bowling_arm, batter_hand, ...)
          │
          ├─► [main.v_batting_totals_season]
          ├─► [main.v_bowling_totals_season]
          ├─► [main.v_batting_ctx_*]
          └─► [main.v_bowling_ctx_*]

7) Validation & reconciliation

Maintain Python validation scripts to assert that each context view sums back to its corresponding totals view for each player-season.

Reconcile counts for:

Batting: runs, balls_faced, outs

Bowling: balls, runs, wickets


