# 💧 BeAware: Water Quality Prediction & Outlier Detection System

BeAware is an AI-powered environmental awareness system designed to monitor, analyze, and predict water quality across the UK. It leverages real-time geolocation and historical data to alert users about pollution levels in rivers, lakes, canals, and groundwater sources.

---

## 🎯 Project Goal

The goal is to build an intelligent system that:
- Detects outliers in water quality parameters
- Predicts real-time and next-day values for key pollutants
- Matches predictions to the user's location and nearest water type
- Displays this information in a user-friendly dashboard

---

## 📊 Features & Visuals

- 🌍 Location-based analysis: match user’s coordinates to the nearest water type
- 📉 Time-based visualizations: show pollution trends by hour, day, month, and season
- 🧭 Outlier Mapping: identify regions with frequent spikes
- 🔮 Real-Time + t+1 Predictions: view predicted values for water quality indicators
- 🧼 Data Insights: explore rolling averages, seasonal patterns, and clusters
- 📌 Streamlit Dashboard: interactive outlier visualization panel

---

## 🧪 Tech Stack

- **Languages**: Python, HTML/CSS/JS (for frontend app), SQL
- **Libraries**: pandas, NumPy, scikit-learn, LightGBM, matplotlib, seaborn
- **Visualization**: Streamlit, GeoPandas, folium
- **Model Management**: joblib for saving and loading models
- **Platform**: Jupyter Notebooks (for experiments), Streamlit (for UI)

---

## 🧼 Data Preprocessing & Feature Engineering

### 🧹 Preprocessing Steps

1. **Filtering & Selection:**
   - Focused on 10 water quality parameters across 243,000+ samples
   - Included only relevant water types: Rivers, Lakes, Groundwater, etc.

2. **Missing Values Handling:**
   - Forward/backward filling within location & parameter
   - Samples with insufficient data were removed

3. **Timestamp Features:**
   - Extracted hour, day, week, month, year
   - Season tagging based on sample date

4. **Outlier Detection:**
   - Used IQR per parameter & water type
   - Added binary outlier flags for each key parameter

5. **Geographic Clustering:**
   - K-Means clustering on coordinates to identify water quality zones

---

### 🛠️ Feature Engineering

- **Rolling Means:** 3/5/7-sample windows for smooth trends
- **Lag Features:** previous values to capture temporal trends
- **Water Type Averages:** used as fallback predictors
- **Outlier Frequency Scores:** how often a site showed spikes

---

## 🧠 Machine Learning Models

- **Model Type:** LightGBM Regressors
- **Separate Models:** Trained per water type & per parameter
- **Prediction Targets:**
  - `Current Value (t)`
  - `Next Sample Value (t+1)`
   

## 📈 Model Performance

The LightGBM regressors were evaluated on two key tasks:
- **Current Value Prediction (t)**
- **Next Sample Forecasting (t+1)**

Performance is reported per **water type** and **parameter** using RMSE and R² scores.

### 🌊 Groundwater

| Parameter           | RMSE (t) | R² (t)    | RMSE (t+1) | R² (t+1)  |
|---------------------|----------|-----------|------------|-----------|
| O Diss %sat         | 6.2616   | 0.9429    | 0.9059     | 0.9967    |
| Orthophospht        | 0.1900   | 0.6132    | 0.7081     | 0.6412    |
| Ammonia(N)          | 0.3627   | 0.8915    | 1.6009     | 0.7845    |
| Temp Water          | 1.2446   | 0.7682    | 0.1555     | 0.9987    |
| pH                  | 0.1559   | 0.9310    | 0.0509     | 0.9900    |
| Nitrite-N           | 0.0579   | 0.5704    | 0.0707     | 0.6710    |
| Nitrate-N           | 1.3951   | 0.9560    | 0.1081     | 0.9994    |
| TurbidityNTU        | 0.3287   | 0.7910    | 20.6912    | 0.7983    |
| BOD ATU             | 0.7283   | 0.7393    | 85.7848    | 0.4378    |
| Phosphorus-P        | 0.0006   | ❌        | 0.5359     | 0.3545    |

---

### 🌊 River / Running Surface Water

| Parameter           | RMSE (t) | R² (t)    | RMSE (t+1) | R² (t+1)  |
|---------------------|----------|-----------|------------|-----------|
| O Diss %sat         | 5.0566   | 0.8766    | 2.4546     | 0.9749    |
| Orthophospht        | 2.5456   | 0.2817    | 2.0405     | 0.5005    |
| Ammonia(N)          | 6.1521   | 0.4757    | 2.6717     | 0.8971    |
| Temp Water          | 1.2050   | 0.9207    | 0.0578     | 0.9998    |
| pH                  | 0.1355   | 0.9020    | 0.0200     | 0.9980    |
| Nitrite-N           | 0.0603   | 0.6649    | 0.0505     | 0.7447    |
| Nitrate-N           | 1.2508   | 0.9172    | 0.4801     | 0.9883    |
| TurbidityNTU        | 14.1410  | 0.8199    | 6.6025     | 0.9591    |
| BOD ATU             | 33.0299  | 0.8031    | 3.4427     | 0.9897    |
| Phosphorus-P        | 0.2709   | 0.7118    | 0.0251     | 0.9972    |

---

### 🌊 Pond / Lake / Reservoir Water

| Parameter           | RMSE (t) | R² (t)    | RMSE (t+1) | R² (t+1)  |
|---------------------|----------|-----------|------------|-----------|
| O Diss %sat         | 8.0341   | 0.8481    | 0.9566     | 0.9962    |
| Orthophospht        | 0.3921   | 0.5698    | 3.6949     | 0.2559    |
| Ammonia(N)          | 2.4494   | 0.4502    | 6.4085     | 0.5019    |
| Temp Water          | 1.5835   | 0.9135    | 0.1424     | 0.9989    |
| pH                  | 0.1903   | 0.8514    | 0.0289     | 0.9959    |
| Nitrite-N           | 0.0033   | 0.7290    | 0.0922     | 0.6045    |
| Nitrate-N           | 0.1423   | 0.9746    | 0.2783     | 0.9960    |
| TurbidityNTU        | 14.1415  | 0.7875    | 17.5790    | 0.5754    |
| BOD ATU             | 6.9122   | 0.8746    | 326.6435   | 0.1100    |
| Phosphorus-P        | 0.0691   | 0.6943    | 0.2377     | 0.7794    |

❌ _Note: Phosphorus-P (Groundwater, t) resulted in undefined/invalid R² due to zero variance in actuals._

---

- **Use Cases:**
  - Real-time prediction
  - Forecasting missing or future measurements
  - Alerting users based on predicted pollution risk

---

## 🗂️ Project Structure
