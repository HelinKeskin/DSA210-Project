# DSA210 Project: Investigating Weather Impacts on Istanbul Traffic Dynamics

**Course:** DSA210 - Introduction to Data Science  
**Semester:** Spring 2026  
**Author:** Helin Keskin  

---

## 1. Introduction & Motivation
Anyone living in Istanbul knows that traffic is a nightmare, but there is a common belief that "whenever it rains, traffic completely paralyzes." The motivation behind this project is to test this popular assumption using actual data. 

By analyzing daily weather variations alongside traffic metrics from January 2025, I aimed to see if factors like precipitation, temperature dips, and cloud cover have a statistically significant effect on daily traffic density, or if the congestion is just driven by standard weekly routines.

## 2. Data Sourcing & Data Prep
For this analysis, I combined two distinct datasets using the daily date as our common key:
- **Weather Metrics:** Hourly weather data was aggregated into daily rows, tracking temperature averages, precipitation probability, actual rainfall amounts, and humidity levels.
- **Traffic Metrics:** Daily records capturing the average traffic intensity percentage, total daily vehicle counts, and categorized congestion states (Medium, High, etc.).

### Cleaning & Merging:
The synchronization was straightforward since both datasets had clean date formats. I handled a few missing entries in the weather features and performed an inner join on the date column to build a final unified dataframe covering the 31 days of January 2025.

## 3. Exploratory Data Analysis (EDA) & Hypothesis Testing
During EDA, plotting the traffic intensity against rainy days showed a visible upward shift in congestion during wet weather. To validate whether this visual trend was actually meaningful or just a coincidence, I set up a hypothesis test.

### Hypothesis Framework:
- **H0 (Null Hypothesis):** The average traffic intensity in Istanbul is the same on rainy days and non-rainy days.
- **Ha (Alternative Hypothesis):** The average traffic intensity is significantly higher on rainy days.

import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestRegressor
from sklearn.metrics import mean_squared_error, r2_score


try:
    weather_df = pd.read_csv('weather_data.csv')
    traffic_df = pd.read_csv('traffic_density_january_2025_daily.csv')
    print("Files loaded successfully.")
except FileNotFoundError:
    print("Error: CSV files not found. Please check the file names.")


if 'datetime' in weather_df.columns:
    weather_df = weather_df.rename(columns={'datetime': 'date'})


weather_df['date'] = pd.to_datetime(weather_df['date'])
traffic_df['date'] = pd.to_datetime(traffic_df['date'])


merged_df = pd.merge(traffic_df, weather_df, on='date')


merged_df = merged_df.dropna(subset=['temp', 'humidity', 'precip', 'windspeed', 'cloudcover', 'traffic_intensity_avg'])

features = ['temp', 'humidity', 'precip', 'windspeed', 'cloudcover']
X = merged_df[features]
y = merged_df['traffic_intensity_avg']


X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

model = RandomForestRegressor(n_estimators=100, random_state=42)
model.fit(X_train, y_train)
y_pred = model.predict(X_test)


print(f"Model R2 Score: {r2_score(y_test, y_pred):.4f}")


plt.figure(figsize=(10, 6))
importances = pd.Series(model.feature_importances_, index=features).sort_values()
importances.plot(kind='barh', color='teal')
plt.title('Impact Level of Weather on Traffic')
plt.savefig('analysis_feature_importance.png')
plt.show()


plt.figure(figsize=(10, 6))
sns.scatterplot(x=y_test, y=y_pred)
plt.plot([y.min(), y.max()], [y.min(), y.max()], '--r')
plt.xlabel('Actual Traffic Intensity')
plt.ylabel('Predicted Traffic Intensity')
plt.title('ML Model Prediction Performance')
plt.savefig('analysis_model_performance.png')
plt.show()


merged_df.to_csv('dsa210_final_output.csv', index=False)
print("Process completed. 'analysis_feature_importance.png', 'analysis_model_performance.png' and 'dsa210_final_output.csv' have been created.")

**Method & Finding:** I ran a two-sample t-test comparing the traffic density distributions. The resulting p-value was low enough (< 0.05) to confidently reject the null hypothesis. This statistically confirms that rainy days do experience worse traffic conditions on average in Istanbul.

## 4. Machine Learning Modeling
Moving past basic correlation, I wanted to see if we could actually *predict* the continuous `traffic_intensity_avg` score using only environmental features: average temperature, precipitation, humidity, and cloud cover.

I split the January data into an 80/20 train-test split and evaluated two models:
1. **Linear Regression:** Served as a baseline to see linear trends.
2. **Random Forest Regressor:** Captured non-linear interactions between weather variables.

### Insights from Evaluation:
While the Random Forest model captured variance slightly better than the linear baseline, the overall R2 scores highlighted an interesting data science lesson: weather is a notable catalyst, but it doesn't tell the whole story. Looking closely at the residuals, the model struggled on certain days—which, upon manual inspection, perfectly lined up with weekends where traffic drops naturally regardless of the rain.

## 5. Reflections & Future Work
This project successfully validated the local intuition: adverse winter weather, particularly rain, genuinely correlates with spiked traffic density in Istanbul. 

However, because the dataset is restricted to January 2025, it heavily reflects winter behavior. If I were to scale this project up, the immediate next steps would be:
1. Expanding the timeline to a full 12-month cycle to see how seasonal rains (like sudden summer downpours) compare to winter patterns.
2. Feature engineering a "Is_Weekend" or "Is_Workday" binary flag into the ML models. This would prevent the algorithm from misinterpreting a rainy Sunday as a high-traffic day, drastically boosting our predictive accuracy.

---
*Note: This repository represents the final submission for the DSA210 course project.*
