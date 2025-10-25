File: metrics_bowling.md

Purpose: Canonical definitions and column order for bowling metrics (season and career aggregations).Source views: main.v_bowling_totals_season (and analogous career view when added).Input basis: staging.v_deliveries_enriched with legality flag is_legal_delivery_bowler (counts legal balls bowled by the bowler).Global exclusions: Exclude super overs (innings 3 and 4) from all metrics.

1) Column order (final)

year, player_name, matches, innings, balls, runs, wickets,
bbi, average, economy, er_per_ball, strike_rate,
fours, sixes, dots, B_percent, D_percent,
three_fers, five_fers

2) Definitions

matches: Count of distinct matches where the player bowled at least one legal ball (innings_number in {1,2}).

innings: Count of bowling innings where the player bowled at least one legal ball.

balls: Count of deliveries where is_legal_delivery_bowler = TRUE for the bowler.

runs: Total runs conceded by the bowler, including boundaries, byes/leg-byes as per your upstream attribution policy (follow the field you designated as “runs conceded by bowler” in the enriched view).

wickets: Count of dismissals credited to the bowler (exclude run-outs not credited to bowler). Follow upstream wicket_type mapping.

bbi (Best Bowling in an innings/match): Record-best wickets-runs figure for a single match (e.g., 5/22). Store as text W/R for display plus numeric fields if desired.

average: runs / wickets if wickets > 0, else NULL.

economy: runs / (balls / 6) using legal balls only (via is_legal_delivery_bowler). If balls = 0, NULL.

er_per_ball: runs / balls (0 if balls = 0 → NULL).

strike_rate: balls / wickets if wickets > 0, else NULL.

fours: Count of fours conceded by the bowler.

sixes: Count of sixes conceded by the bowler.

dots: Count of legal deliveries conceded for 0 runs (by batter and extras policy; align with your upstream dot logic).

B_percent (Boundary % conceded): boundary_runs_conceded / runs where boundary_runs_conceded = 4*fours + 6*sixes. If runs = 0, NULL.

D_percent (Dot %): dots / balls. If balls = 0, NULL.

three_fers: Count of innings with exactly 3 wickets.

five_fers: Count of innings with ≥5 wickets.

3) Counting & legality rules

Use is_legal_delivery_bowler for all bowling ball counts.

Super overs excluded globally (innings 3–4).

Economy uses only legal balls (balls / 6).

4) Reconciliation expectations

For any context breakdown (vs batter hand, by over, by innings, by position, by venue, by result), aggregated balls, runs, wickets must sum exactly to the season totals for each player.
