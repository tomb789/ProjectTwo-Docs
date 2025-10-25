Hundred Cricket Database — Master Plan (Lite)
Python 3.11 + DuckDB + Power BI | IDE: PyCharm

Purpose:
Transform Cricsheet YAML → DuckDB → Power BI analytics warehouse.

Core rules:
- Verify, don’t guess; schema = source of truth.
- Modular, idempotent pipeline (staging → dims → derived → facts → exports).
- Explicit open/close DuckDB connections.
- lower_snake_case, *_id = key, *_name = label.
- Validation and logging per phase.

Layers:
stg_* → raw parsed YAML (matches, innings, deliveries, players)
dim_* → players, teams, venues, phase schemes
v_*  → enriched deliveries, inferred positions, orders
metrics layer → batting/bowling totals & context views
fact_* → materialised season stats for Power BI

Workflow:
YAML → parse_yaml_to_staging.py
→ validate_stg_*.py
→ build_dimensions.py
→ build_v_deliveries_enriched.sql
→ build_metrics_views.py / build_context_views.py
→ export_facts_to_parquet.py → outputs/exports/<timestamp>
→ Power BI dashboards

Validation highlights:
- All FK/PK valid
- balls_per_over ∈ {5,6}, wickets ≤10
- Exclude innings 3–4 (super overs)
- Sum(batter + extras) = innings total
- One phase per delivery
- No null hands/styles in player enrichment
- Parquet counts = DB facts

Databases:
db/staging.db (raw)
db/metrics.db (analytics)

Reports: /reports/
Exports: /outputs/exports/
Power BI: /PowerBI/

Phase roadmap:
1 Ingestion — ✅
2 Dimensions — ✅
3 Metrics foundation — 🚧 active
4 Automation — 🔜
5 Advanced context & modelling — 🔮
