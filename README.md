# Nectar Data Scientist Challenge — Submission

## Structure
```
Nectar_Data_Scientist_Challenge/
├── README.md
├── requirements.txt
├── notebooks/
│   └── Nectar_Data_Scientist_Challenge.ipynb   # all 5 tasks, already run with outputs
├── data/
│   ├── sensor_telemetry.csv                    # 73,440 hourly readings, 51 assets
│   ├── asset_metadata.csv                      # asset hierarchy + specs
│   └── asset_connectivity.csv                  # asset-to-asset connections
└── report/
    └── Nectar_Challenge_Report.pptx             # 5-slide summary deck
```

## Setup
```bash
pip install -r requirements.txt
jupyter notebook notebooks/Nectar_Data_Scientist_Challenge.ipynb
```
Run all cells top to bottom. The first section of the notebook generates the three
CSVs into `data/` — they're already included here so the notebook can also be run
starting from the EDA section directly against the provided files.

## Why synthetic data
No real dataset was provided with the challenge, so I generated one that matches the
schema described in the brief (sensor telemetry, asset metadata, asset connectivity).
Assumptions made during generation are documented in the notebook itself, right
before the generation code.

## Data dictionary (matches the challenge brief's schema)
- **sensor_telemetry.csv** — `timestamp, site_id, building_id, asset_id, temperature,
  humidity, pressure, vibration, power_consumption, occupancy_count, operating_mode,
  fault_flag`
- **asset_metadata.csv** — `asset_id, site_id, asset_name, asset_type, manufacturer,
  installation_date, capacity, parent_asset_id`
- **asset_connectivity.csv** — `source_asset_id, target_asset_id, connection_type,
  relationship_strength`

## Architecture / approach
- **EDA:** pandas/seaborn, distribution + correlation analysis, comparisons across
  sites and asset types.
- **Predictive maintenance:** XGBoost classifier predicting fault-within-24h, using
  rolling statistics and short-term trend features. Random Forest included as a
  baseline comparison.
- **Energy forecasting:** XGBoost regressor with lag + calendar features, 24h-ahead,
  per building.
- **Anomaly detection:** Isolation Forest (per asset type) plus a z-score check and a
  rolling-vs-baseline drift check.
- **Connectivity analysis:** `networkx` directed graph built from the asset
  hierarchy, used for downstream-impact and data-quality checks.

## Design decisions / trade-offs
- Kept everything in one notebook rather than splitting into a package + API, since
  the brief's core ask is the analysis and modelling, not a deployed service.
- Time-based train/test split for both ML tasks (not random), to avoid leaking future
  information into training.
- Full reasoning, metric results, and business-impact notes for each task are written
  directly in the notebook next to the relevant code, rather than duplicated here.

## Limitations / next steps
See the "What I'd do with more time" note at the end of the notebook, and the
recommendations slide in `report/Nectar_Challenge_Report.pptx`.
