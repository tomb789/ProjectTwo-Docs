Hundred Cricket Database — Metrics Charter (Lite)

General:
All metrics derive from v_deliveries_enriched.
Grain = year × player_name × team_name.
Percentages float (0–100), round 2dp.
Super overs (innings 3–4) excluded.

Batting metrics:
matches = COUNT(DISTINCT match_id)
innings = COUNT_IF(balls_faced>0)
runs = SUM(runs_batter_credit)
balls_faced = COUNT_IF(is_legal OR extras_type='noball')
outs = COUNT_IF(player_out_name=batter_name)
not_outs = innings−outs
average = runs/outs
strike_rate = 100*runs/balls_faced
HS = MAX(runs_in_innings)
fours = COUNT_IF(runs_batter=4)
sixes = COUNT_IF(runs_batter=6)
dots = COUNT_IF(runs_batter=0 AND is_legal)
boundary_runs = 4*fours+6*sixes
B% = 100*boundary_runs/runs
BB% = 100*(fours+sixes)/balls_faced
D% = 100*dots/balls_faced
50s = COUNT_IF(runs_in_innings BETWEEN 50 AND 99)
100s = COUNT_IF(runs_in_innings>=100)

Bowling metrics:
matches = COUNT(DISTINCT match_id)
innings = COUNT_IF(balls>0)
balls = COUNT_IF(is_legal)
runs = SUM(runs_conceded_bowler)
wickets = COUNT_IF(valid_wicket)
average = runs/wkts
economy = runs/(balls/balls_per_over)
er_per_ball = runs/balls
strike_rate = balls/wkts
fours/sixes/dots same logic as batting
B% = 100*boundary_runs/runs
D% = 100*dots/balls
three_fers = COUNT_IF(3≤wkts<5)
five_fers = COUNT_IF(wkts≥5)

Context tables:
Batting: vs_spin_seam, by_bowling_style, vs_hand, by_position, by_over, by_innings, by_result, by_venue
Bowling: vs_batter_hand, by_over, by_innings, by_batter_position, by_result, by_venue

Extras policy:
No-ball: team✅ bowler✅ legal❌
Wide: team✅ bowler✅ legal❌
Bye/Leg-bye/Penalty: team✅ bowler❌ legal✅
Overthrows: depends

Exclusions:
Super overs, retired hurt/run out/obstructing, abandoned matches.

Outputs:
v_batting_totals_season
v_bowling_totals_season
v_batting_by_phase
v_bowling_by_phase
fact_batting_totals
fact_bowling_totals
