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
