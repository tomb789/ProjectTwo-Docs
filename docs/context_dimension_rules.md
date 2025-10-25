# Context Dimension Rules (Authoritative)
Version: v1.0 • Last updated: 2025-10-25

## Purpose
Rules for every contextual breakdown used in batting and bowling analytics.

Each context table must:
1. Be derived from `staging.v_deliveries_enriched`
2. Apply correct legality flags
3. Exclude super overs (innings 3–4)
4. Reconcile exactly to season totals per player

---

## Common Principles
- Use the same legality flags as totals:
  - Batting: `is_ball_faced_batter`
  - Bowling: `is_legal_delivery_bowler`
- Include season (`year`) and `player_name` in every context table
- Round percentages at presentation time
- Maintain consistent naming and column order (player-first)

---

## Batting Contexts

### 1️⃣ vs spin or seam
Group by bowler style → Normalisation map required:
- spin
- seam

### 2️⃣ by bowling style
Group by full style string (e.g. Right-arm legbreak)

### 3️⃣ by bowling arm
- Left-arm
- Right-arm

### 4️⃣ by batting position
Position inferred using the permanent rule:
- Derived from Cricsheet batter entry events (`batter`, `non_striker`)
- Positions 1–11 only

### 5️⃣ by over number
Over 1 to 20
No pre-bucketing unless explicitly defined elsewhere

### 6️⃣ by match innings
- 1 or 2 only

### 7️⃣ by venue
Format: `ground, city`
Keep venue fields consistent with `matches` table

### 8️⃣ by match result
Perspective: batting team result  
- win / loss / tie / no result

---

## Bowling Contexts

### 1️⃣ vs batter hand
Left / Right batting hand

### 2️⃣ by over number
Legal overs bowled, 1 to 20

### 3️⃣ by match innings
1 or 2 only

### 4️⃣ by batter position
Batter’s position at time of delivery  
(Use same rule as batting contexts)

### 5️⃣ by match result
Perspective: bowler’s team result

### 6️⃣ by venue
Same `ground, city` convention

---

## Reconciliation Requirements

For Batting:
- runs
- balls_faced
- outs

For Bowling:
- balls
- runs
- wickets

All must sum exactly to the totals view outputs for:
`main.v_batting_totals_season` and `main.v_bowling_totals_season`

---

## Status
Authoritative and enforced.
