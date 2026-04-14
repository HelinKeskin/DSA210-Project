# 🚦 DSA 210 — Istanbul Traffic Density & Weather Conditions

**Student:** Helin Keskin  
**University:** Sabancı University  
**Course:** DSA 210 — Introduction to Data Science  
**Period Analyzed:** January 2025  

---

## 📌 Project Overview

This project investigates the **correlation between Istanbul Traffic Density and Weather Conditions** (Temperature, Precipitation, Humidity) for January 2025.

**Research Question:** *Does rain significantly increase traffic density in Istanbul?*

---

## 📂 Data Sources

| Dataset | Source | Description |
|---|---|---|
| `traffic_data.csv` | Istanbul Metropolitan Municipality (IMM) Open Data | Hourly traffic speed & vehicle counts by road segment |
| `weather_data.csv` | Visual Crossing Weather API | Daily weather conditions (temp, precip, humidity) |

---

## 🔬 Methodology

The analysis follows a complete data science pipeline:

1. **Data Loading & Enrichment** — Loaded both CSVs, converted timestamps, filtered for January 2025, interpolated missing values, and merged datasets on the `date` column.
2. **Exploratory Data Analysis (EDA)** — Three key visualizations: correlation heatmap, 24-hour traffic cycle line chart, and weather event boxplot.
3. **Statistical Hypothesis Testing** — Independent T-Test (one-tailed) via `scipy.stats`.

---

## 📊 Key Results

### EDA Findings

**Correlation Heatmap**
The heatmap revealed that `precip` (precipitation) and `NUMBER_OF_VEHICLES` have a slight negative correlation, suggesting rainy days do not meaningfully increase vehicle counts in the analyzed period.

**24-Hour Traffic Cycle (Rainy vs Clear)**
Traffic peaks occur during morning rush (07:00–09:00) and evening rush (17:00–19:00) on both rainy and clear days. The overall pattern is similar across weather conditions.

**Traffic Occupancy Boxplot (Rain / Snow / Sunny)**
The distribution of vehicle counts across weather event types shows overlapping interquartile ranges, indicating weather type alone is not a strong predictor of traffic occupancy.

---

## 📐 Hypothesis Test

| | |
|---|---|
| **H₀ (Null)** | No significant difference in traffic density between rainy and non-rainy hours |
| **H₁ (Alternative)** | Traffic density is significantly higher during rainy hours |
| **Method** | Independent T-Test (one-tailed), α = 0.05 |

### Results

| Metric | Value |
|---|---|
| Rainy Mean Vehicles | 85.56 |
| Clear Mean Vehicles | 91.42 |
| T-statistic | -34.22 |
| P-value | 1.0000 |

**Decision: ✅ Fail to Reject H₀**

> The p-value (1.0) is far above the significance threshold (α = 0.05). There is **no statistically significant increase** in traffic density during rainy conditions in Istanbul for January 2025. In fact, average vehicle count was slightly *lower* on rainy days (85.56) compared to clear days (91.42), suggesting drivers may avoid travel during precipitation.

---

## ⚖️ Ethical Considerations

- **Data Privacy:** Both datasets are open-access with no personally identifiable information (PII).
- **Bias Awareness:** Results are limited to January 2025 and may not generalize to other seasons.
- **Causality Warning:** Correlations observed do not imply causation. Holidays, events, and road construction are confounding factors.

---

## 🗂️ Repository Structure

```
DSA210-Project/
├── data/
│   ├── weather_data.csv      # Daily weather data (Visual Crossing)
│   └── traffic_data.csv      # Hourly traffic data (IMM) — not tracked in git (large file)
├── notebooks/
│   └── analysis.ipynb        # Full Jupyter Notebook with all analysis steps
├── requirements.txt          # Python dependencies
└── README.md                 # This file
```

---

## 🚀 Running the Project

```bash
# Install dependencies
pip install -r requirements.txt

# Launch Jupyter Notebook
jupyter notebook notebooks/analysis.ipynb
```

---

## 📦 Dependencies

See [`requirements.txt`](requirements.txt):
- `pandas` — data manipulation
- `numpy` — numerical operations
- `matplotlib` — plotting
- `seaborn` — statistical visualization
- `scipy` — hypothesis testing
- `jupyter` — notebook environment
