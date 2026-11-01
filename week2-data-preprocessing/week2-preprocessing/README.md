# AirCast India — Week 2: Data Preprocessing & Feature Engineering

**Author:** Ansh Parashar | **Internship Week 2 Task**

---

## Overview

A complete, reproducible data preprocessing and feature engineering pipeline for the **AirCast India** air-quality forecasting project. The notebook processes hourly PM2.5 and meteorological data for 8 Indian cities and prepares a feature-rich dataset ready for model training.

## Repository Structure

```
.
├── AirCast_India_Preprocessing.ipynb   ← Main deliverable (run this)
├── requirements.txt
└── README.md
```

Output files generated on run:
- `train_processed.csv`, `test_processed.csv` — processed datasets
- `fig_01_pm25_hist.png` … `fig_07_clean_ts.png` — all visualisations

## How to Run

```bash
pip install -r requirements.txt
jupyter notebook AirCast_India_Preprocessing.ipynb
```

Then: **Kernel → Restart & Run All**

No API keys required — data is generated with a fixed random seed (`SEED = 42`) for full reproducibility.

## Pipeline Steps

| # | Step | Method |
|---|------|--------|
| 1 | Data loading | Simulated OpenAQ/CPCB schema (365 days × 8 cities × hourly) |
| 2 | EDA | Histograms, box plots, time-series plots |
| 3 | Duplicate removal | `drop_duplicates()` |
| 4 | Invalid-value handling | Clip PM2.5 ≤ 0 or > 500 µg/m³ → NaN |
| 5 | Missing-value imputation | KNN Imputer (k=5) per city |
| 6 | Outlier treatment | Winsorise at 99th percentile per city |
| 7 | Temporal features | Cyclic sin/cos encoding, season, is_weekend |
| 8 | Lag & rolling features | Lags: 1 h, 3 h, 6 h, 12 h, 24 h; Rolling: 3/6/24 h mean + std |
| 9 | Meteorological features | Ventilation index, heat index, stagnant-air flag |
| 10 | Target engineering | 24-h ahead PM2.5; CPCB AQI category |
| 11 | Categorical encoding | One-hot (season), ordinal (city, AQI) |
| 12 | Scaling | StandardScaler (meteo) + MinMaxScaler (lags), train-set fit only |

## Visualisations Produced

1. PM2.5 histograms — raw data, all 8 cities
2. Box plots — PM2.5 by city
3. Time-series — Delhi raw hourly
4. Scatter — before vs after cleaning (Delhi)
5. AQI category distribution bar chart
6. Feature correlation heatmap
7. Cleaned time-series with 24-h rolling mean

## Requirements

See `requirements.txt`. Core dependencies: `pandas`, `numpy`, `matplotlib`, `seaborn`, `scikit-learn`, `jupyter`.

---

*Part of the AirCast India AI project — predicting next-day PM2.5 for 8 Indian cities.*
