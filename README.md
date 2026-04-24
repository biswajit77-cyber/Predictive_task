# 📊 Power Demand Forecasting using Random Forest

## 📌 Overview

This project forecasts electricity demand (MW) from historical data by combining weather conditions, economic indicators, and past demand values. A **Random Forest Regressor** was trained on time-series features engineered to capture both short-term trends and seasonal patterns.

---

## 📂 Dataset

Three data sources were merged on a common datetime index:

| File | Description |
|------|-------------|
| `economic_full_1.csv` | Economic indicators |
| `weather_data.xlsx` | Weather conditions |
| `PGCB_date_power_demand.xlsx` | Historical power demand (MW) |

---

## ⚙️ Data Preprocessing

- Converted date columns to `datetime` format
- Filtered data within a relevant time range
- Merged all datasets on a unified timestamp index
- Handled missing values using **forward fill**
- Dropped columns with excessive missing values
- Removed **low-variance features**

---

## 🧠 Feature Engineering

### 📅 Time-Based Features
`Year` · `Month` · `Day` · `Hour`

### 🔁 Lag Features
- `lag1` — previous hour demand
- `lag24` — previous day demand

### 📊 Rolling Statistics
- `rolling_mean_24` — 24-hour moving average

### 🔄 Cyclical Encoding
To preserve the circular nature of time:
- `sin(hour)` and `cos(hour)`

### 🔍 Feature Selection
- Kept features with **correlation > 0.3** with the target (`demand_mw`)
- Removed highly correlated feature pairs to reduce redundancy

---

## 🤖 Model

```python
RandomForestRegressor(
    n_estimators=300,
    random_state=42,
    n_jobs=-1
)
```

---

## 🏋️ Training & Evaluation

Two training strategies were tested to validate generalization:

### ✅ Experiment 1 — Short-Term Forecast
- **Train:** 2019 data
- **Test:** 2020 data
- **R² Score: ~0.96**
- Demonstrates the model captures short-term demand patterns effectively.

### ✅ Experiment 2 — Long-Term Generalization
- **Train:** All data before 2023
- **Test:** 2024 data
- **Result:** Strong generalization on unseen future data
- Validates real-world forecasting readiness.

---

## 📈 Evaluation Metrics

- **MAE** — Mean Absolute Error
- **RMSE** — Root Mean Squared Error
- **R²** — Coefficient of Determination

Actual vs. predicted demand was also plotted for visual comparison.

---

## 🚀 How to Run

1. **Install dependencies:**

```bash
pip install pandas numpy scikit-learn matplotlib openpyxl
```

2. **Launch the notebook:**

```bash
jupyter notebook
```

3. Open the `.ipynb` file and run cells sequentially.

---

## 🧩 Future Improvements

- [ ] Hyperparameter tuning with `GridSearchCV` or `Optuna`
- [ ] Try gradient boosting models (`XGBoost`, `LightGBM`)
- [ ] Improve outlier detection and handling
- [ ] Implement proper time-series cross-validation
- [ ] Explore deep learning approaches (`LSTM`, `Transformer`)

---

## 👤 Author

**Biswajit Patir**
