# Philippine WESM Telemetry Dataset Documentation

## 1. Overview & Data Provenance
* **Data Owner:** Independent Electricity Market Operator of the Philippines (IEMOP)
* **Market Framework:** Wholesale Electricity Spot Market (WESM) established under Republic Act No. 9136 (EPIRA)
* **Dataset Type:** Real-Time Dispatch Regional Summaries (`RTDREG`)
* **Temporal Resolution:** Five-minute market dispatch intervals (288 intervals per operating day)
* **Time Span:** June 4, 2026 to September 1, 2026 (90 continuous operating days)
* **Regional Grids Covered:**
  * `CLUZ`: Luzon Grid
  * `CVIS`: Visayas Grid
  * `CMIN`: Mindanao Grid

---

## 2. Directory Contents & Structure
```text
Data/
├── README.md                      # Dataset documentation and schema guide (This file)
├── wesm_rtd_90days_energy.csv     # Consolidated 90-day Energy dataset (77,757 rows x 12 columns)
└── raw/                           # All 90 raw daily CSV files (RTDREG_20260604.csv to RTDREG_20260901.csv)
```

---

## 3. Data Dictionary (`wesm_rtd_90days_energy.csv`)

| Column Name | Data Type | Physical Meaning | Analytical Role |
|:---|:---|:---|:---|
| `TIME_INTERVAL` | Datetime | Five-minute dispatch timestamp (e.g., `2026-06-04 00:05:00`) | Chronological Time Index |
| `REGION_NAME` | String | Regional electrical grid (`CLUZ`, `CVIS`, `CMIN`) | Categorical Grouping Identifier |
| `COMMODITY_TYPE`| String | Market product code (`En` = Delivered Electrical Energy) | Filter Filter |
| `MKT_REQT` | Float (MW) | Total regional customer market requirement / load demanded | Primary Demand Metric & Feature |
| `LOAD_BID` | Float (MW) | Aggregate load bidding volume submitted by market participants | Predictor Feature |
| `GENERATION` | Float (MW) | Total energy cleared and dispatched from regional generators | Target Formulation / Predictor |
| `LOAD_CURTAILED`| Float (MW) | Involuntary emergency load curtailment (0.0 during normal operations) | Operational Diagnostic Metric |
| `LOSSES` | Float (MW) | Transmission line electrical heat losses across the regional grid | Grid Physical Strain Predictor |
| `MKT_IMPORT` | Float (MW) | Power imported into the region via high-voltage interties / cables | Intertie Flow Predictor |
| `MKT_EXPORT` | Float (MW) | Power exported out of the region to neighboring grids | Intertie Flow Predictor |
| `RUN_TIME` | String | Market optimization engine execution timestamp | Telemetry Metadata |

---

## 4. Derived Features for One-Hour-Ahead Forecasting

In the machine learning forecasting pipeline, the following time-lagged and rolling variables are engineered:
* **`Supply_Margin`**: $\text{GENERATION} - \text{MKT\_REQT}$ (Net local generation cushion in MW)
* **`Alert_Level`**: Traffic Light alert classification ($0 = \text{Normal } [\ge 650\text{ MW}]$, $1 = \text{Yellow } [0\text{--}650\text{ MW}]$, $2 = \text{Red } [\le 0\text{ MW}]$)
* **`Target_Alert_1hr`**: Forward alert label shifted 12 intervals into the future ($t + 12 = 60\text{ minutes}$)
* **`Net_Intertie`**: $\text{MKT\_IMPORT} - \text{MKT\_EXPORT}$ (Net power exchange flow)
* **`MKT_REQT_lag1`**: Market requirement 5 minutes prior ($t - 1$)
* **`MKT_REQT_lag12`**: Market requirement 60 minutes prior ($t - 12$)
* **`Demand_roll_mean_12`**: 1-hour trailing rolling mean of customer demand
* **`Demand_roll_std_12`**: 1-hour trailing rolling volatility (standard deviation) of demand
* **`Hour_sin` / `Hour_cos`**: Continuous trigonometric cyclical representations of hour of day ($[0, 23]$)

---

## 5. Official Source & Acquisition
Data can be directly downloaded from the **IEMOP Market Data Portal**:
* Portal: [https://www.iemop.ph/market-data/](https://www.iemop.ph/market-data/)
* Category: *Real-Time Dispatch (RTD) Regional Summaries*
* Filter: Select target operating dates and download daily CSV packages.
