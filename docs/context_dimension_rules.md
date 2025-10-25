File: context_dimension_rules.md

Purpose: Stable rules for all context breakdowns used by batting and bowling views/tables.Scope: Naming, grouping, derivation, and reconciliation requirements.

1) Common principles

All context tables must reconcile exactly to their corresponding totals view for each player and season (batting: runs, balls_faced, outs; bowling: balls, runs, wickets).

Apply the same legality flags used in totals: is_ball_faced_batter for batting, is_legal_delivery_bowler for bowling.

Super overs always excluded (innings 3–4).

Column order and naming must follow the Metrics Charter and dashboard expectations.

2) Batting contexts

vs spin or seam: Map bowler styles to spin vs seam. Maintain a single source-of-truth style map in code or reference table.

by bowling style: Group by the bowler’s full bowling style string (normalized; e.g., Right-arm legbreak).

by bowling arm: Group by left-arm vs right-arm of the bowler.

by position: Batting position inferred from deliveries using the permanent rule: derive batting order via batter entry events from Cricsheet fields batter and non_striker (your saved project rule). Positions 1–11 only.

by over number: Over 1 to 20; do not pre-bucket unless a separate “phase” table is desired. If you do define phases, document mapping explicitly (e.g., 1–6 Powerplay, 7–15 Middle, 16–20 Death).

by match innings: 1 or 2 only.

by venue: Output as ground, city per your dashboard preference. Keep a reference table for ground and city in matches to avoid free-text drift.

by match result: Use team result from matches joined to the batting team for that innings: win, loss, tie, no result.

3) Bowling contexts

vs batter hand: Group by batter batting hand left / right.

by over number: Legal overs bowled, 1–20.

by match innings: 1 or 2 only.

by batter position: Batter’s position at time of facing the ball (1–11), derived from the same permanent rule as batting contexts.

by match result: Result from perspective of the bowler’s team for that match/innings.

by venue: ground, city formatting, referencing the same venue table.

4) Display & naming rules

Always include year and player_name as leading columns in context tables.

Include an innings column where applicable, as you specified for certain contexts, to aid grouping in dashboards.

Ensure all percentage fields are rounded to 2 decimals at presentation time; store as decimals (not strings) in intermediate outputs.


