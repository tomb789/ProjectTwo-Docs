File: metrics_batting.md

Purpose: Canonical definitions and column order for batting metrics (season and career aggregations).Source views: main.v_batting_totals_season (and analogous career view when added).Input basis: staging.v_deliveries_enriched with legality flag is_ball_faced_batter (counts legal balls faced for batting).Global exclusions: Exclude super overs (innings 3 and 4) from all metrics.

1) Column order (final)

year, player_name, matches, innings, runs, balls_faced,
outs, not_outs, average, strike_rate,
HS, fours, sixes, dots, B_percent, D_percent,
fifties, hundreds

2) Definitions

matches: Count of distinct matches where the player batted at least once (innings_number in {1,2}).

innings: Count of batting innings for the player (innings where is_ball_faced_batter = TRUE or a dismissal recorded for the player).

runs: Sum of runs_batter_credit across legal batting deliveries for the player.

balls_faced: Count of deliveries where is_ball_faced_batter = TRUE for the player. Wides do not count; no-balls count as balls faced only if a legal ball was delivered (follow your upstream flag’s logic).

outs: Number of innings where the batter was dismissed (any player_out_name = player_name).

not_outs: innings - outs.

average: runs / outs if outs > 0, else NULL (display as —).

strike_rate: 100 * runs / balls_faced (uses is_ball_faced_batter).

HS (High Score): Max runs in a single innings. If not out in that innings, conventional notation is e.g. 72* for display; the stored numeric is 72.

fours: Count of boundary fours hit by the batter.

sixes: Count of boundary sixes hit by the batter.

dots: Count of balls faced where the batter scored 0 off the bat (deliveries with runs_batter_credit = 0, limited to is_ball_faced_batter = TRUE).

B_percent (Boundary %): boundary_runs / runs where boundary_runs = 4*fours + 6*sixes. If runs = 0, return NULL (display 0% only if you explicitly want that policy).

D_percent (Dot %): dots / balls_faced. If balls_faced = 0, NULL.

fifties: Count of innings with 50–99 runs.

hundreds: Count of innings with ≥100 runs.

3) Counting & legality rules

Use is_ball_faced_batter for all batting ball counts.

Super overs excluded globally (innings 3–4).

Wides never count as balls faced; no-balls follow upstream flag logic in is_ball_faced_batter.

4) Reconciliation expectations

For any context breakdown (vs spin/seam, by position, by over, by innings, by venue, by result, vs bowling hand), aggregated runs, balls_faced, outs must sum exactly to the season totals for each player.
