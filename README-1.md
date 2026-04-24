# Power Demand Forecasting using Random Forest

## What this project is about

I wanted to see if I could predict electricity demand (in MW) using historical data. The idea was simple — power demand doesn't just depend on time, it's also tied to weather and broader economic conditions. So I pulled in data from all three sources, merged them, and trained a Random Forest model on top of engineered time-series features.

It worked better than I expected, hitting an R² of ~0.96 in one of the experiments.

---

## Data

I used three separate datasets and merged them on a common datetime index:

1. `economic_full_1.csv` — economic indicators
2. `weather_data.xlsx` — weather conditions
3. `PGCB_date_power_demand.xlsx` — actual power demand readings

The merging step was a bit messy since the timestamps weren't always aligned perfectly, but forward fill handled most of the gaps.

---

## Preprocessing

Nothing too exotic here:

a. Parsed and standardized all date columns
b. Filtered to a relevant time window
c. Forward-filled missing values
d. Dropped columns that were mostly empty or had near-zero variance

---

## Feature Engineering

This is where most of the work happened. Raw timestamps don't mean much to a model, so I created:

1. **Lag features** — `lag1` (last hour) and `lag24` (same hour yesterday)
2. **Rolling average** — 24-hour window to smooth out noise
3. **Cyclical encoding** — `sin` and `cos` of the hour, so the model understands that hour 23 and hour 0 are close together
4. Basic time components — year, month, day, hour

For feature selection, I kept anything with a correlation above 0.3 with the target and dropped features that were too correlated with each other.

---

## Model

Just a Random Forest — no fancy tuning yet:

```python
RandomForestRegressor(
    n_estimators=300,
    random_state=42,
    n_jobs=-1
)
```

---

## Experiments

I ran two separate train/test splits to check how the model holds up over time.

**Experiment 1** — trained on 2019, tested on 2020  
R² came out to ~0.96, which was honestly better than I expected for a year-ahead forecast.

**Experiment 2** — trained on everything before 2023, tested on 2024  
The model still generalized well, which gave me more confidence it's picking up real patterns and not just memorizing the training set.

Metrics used: MAE, RMSE, and R². Also plotted actual vs predicted demand to sanity-check the results visually.

---

## How to run it

```bash
pip install pandas numpy scikit-learn matplotlib openpyxl
jupyter notebook
```

Open the notebook and run cells top to bottom. Make sure the three data files are in the same directory.

---

## What I'd do differently next time

1. Tune the hyperparameters properly (haven't tried Optuna yet but it's on the list)
2. Test XGBoost or LightGBM — Random Forest did well but boosting models usually edge it out on tabular data
3. Better outlier handling in preprocessing
4. Proper time-series cross-validation instead of a single train/test split
5. Maybe try an LSTM if I want to go the deep learning route

---

## Author

Biswajit Patir
