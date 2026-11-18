# Philippine WESM: Regional Supply-Demand Analysis & 1-Hour-Ahead Forecasting

**An end-to-end machine learning project using 90 days of Philippine electricity market data.**

Can historical electricity market data help identify changes in regional supply-demand conditions one hour ahead?

This project explores that question using five-minute Real-Time Dispatch (RTD) data from the **Wholesale Electricity Spot Market (WESM)**, covering Luzon, Visayas, and Mindanao.

The goal is to turn raw market data into understandable regional insights and an experimental machine learning model that forecasts supply-demand conditions 60 minutes ahead.

> **Important:** The traffic-light categories in this project are analytical labels created using an assumed supply-margin threshold. They are not official NGCP grid alerts, and the model is not designed or validated for live grid operations.

## Project at a Glance

| Item | Description |
|---|---|
| **Domain** | Philippine electricity market |
| **Data source** | IEMOP WESM Real-Time Dispatch data |
| **Coverage** | 90 days, June 4–September 1, 2026 |
| **Time interval** | Five-minute observations |
| **Regions** | Luzon, Visayas, and Mindanao |
| **Forecast horizon** | 60 minutes ahead |
| **Models** | Logistic Regression and Random Forest |
| **Evaluation** | Chronological train-test split |

## Why This Project Matters

Electricity supply and demand must be balanced continuously. When demand approaches or exceeds locally generated supply, the resulting conditions deserve closer attention.

This project investigates whether patterns in historical demand, generation, regional power exchanges, and time of day can help identify potentially constrained supply conditions in advance.

The intended value is **earlier analytical visibility**: helping people explore patterns and assess conditions, rather than replacing decisions made by authorized grid operators.

## What I Built

The project brings together four main steps:

1. **Data preparation** — cleaned and organized five-minute market records by region and timestamp.
2. **Exploratory analysis** — examined regional demand, generation, supply margins, and time-based patterns.
3. **Target and feature engineering** — created project-defined traffic-light categories and forecasting features such as lagged demand, rolling averages, net intertie flow, and time-of-day patterns.
4. **Model development and evaluation** — trained and compared Logistic Regression and Random Forest using a chronological test period.

### How the Traffic-Light Labels Work

I calculated a simplified regional supply margin:

`Supply_Margin = GENERATION - MKT_REQT`

The project then uses the following assumed thresholds:

| Project label | Supply margin |
|---|---:|
| 🟢 **Normal** | 650 MW or higher |
| 🟡 **Yellow** | Above 0 MW and below 650 MW |
| 🔴 **Red** | 0 MW or lower |

These categories simplify the data for analysis. They do not account for every factor involved in actual grid alert declarations, such as reserve requirements and transmission conditions.

## Model Results

The models were evaluated on a later, unseen period: **August 1–September 1, 2026**.

| Model | Accuracy | Macro F1-score |
|---|---:|---:|
| **Logistic Regression** | **99.29%** | **85.37%** |
| Random Forest | 99.17% | 69.14% |

Logistic Regression performed better on both reported measures.

Accuracy shows the overall share of correct predictions. Macro F1 gives a more balanced view across the three classes, which is especially important when some categories appear much less often than others.

### A Closer Look at the Logistic Regression Results

| Class | Test observations | Recall |
|---|---:|---:|
| Normal | 95 | 51.58% |
| Yellow | 18,346 | 99.43% |
| Red | 9,174 | 99.50% |

**An important finding:** The Normal class had only 95 test observations, and its recall was lower than the other classes. This highlights why accuracy alone is not enough to evaluate a model when the data is imbalanced.

The results are promising for further study, but they do not establish that the model is reliable enough for operational decisions.

## Tools & Technologies

- **Python** — project development
- **Pandas and NumPy** — data preparation and feature engineering
- **Matplotlib and Seaborn** — data visualization
- **SciPy** — statistical analysis
- **Scikit-learn** — machine learning and model evaluation
- **Jupyter Notebook** — analysis and documentation

## Explore the Project

The main notebook contains the complete analysis, visualizations, preprocessing steps, and model evaluation:

**[`Philippine_WESM_Grid_Alert_Classification.ipynb`](Philippine_WESM_Grid_Alert_Classification.ipynb)**

### Run Locally

```bash
# Clone the repository
git clone https://github.com/EJTeope/WESM-Electricity-Dispatch-Analysis.git
cd WESM-Electricity-Dispatch-Analysis

# Create and activate a virtual environment
python -m venv venv

# Windows
venv\Scripts\activate

# macOS / Linux
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Launch the notebook
jupyter notebook
```

See `Data/README.md` for dataset information and instructions.

## Limitations & Next Steps

This is an experimental analytical project, not a production forecasting or grid-control system.

Future improvements include testing on additional months of data, investigating the limited number of Normal observations, validating the chosen thresholds with electricity-sector experts, and assessing whether the model's inputs would be available consistently at prediction time.

## Author

**Erden Jhed D. Teope**  
BS Information Systems — Major in Business Analytics  
West Visayas State University, College of ICT  
Iloilo City, Philippines

[LinkedIn](https://www.linkedin.com/in/ejteope/) · [GitHub](https://github.com/EJTeope)
