# 💧 Water Quality Prediction (2020–2025)

This project aims to analyze, model, and forecast water pollution levels across England between 2020 and 2025. It uses historical environmental monitoring data, advanced data preprocessing, feature engineering, and machine learning to predict the values of 10 key water quality parameters.

---

## 🎯 Project Goal

To build a reliable predictive system that:
- 📍 Identifies the nearest water type from a user's location
- 🔮 Predicts the current and next-day pollution levels for 10 water parameters
- 🌍 Powers the backend of the upcoming **BeAware** environmental awareness app
- 📊 Enables real-time environmental forecasting based on user coordinates

---

## 🔍 Data Overview

- **Source**: UK Government environmental datasets (2020–2025)
- **Samples**: 243,000+ water samples
- **Columns**: Timestamp, water type, location coordinates, pollution parameters
- **Key Parameters**:
  1. O Diss %sat
  2. Orthophosphates
  3. Ammonia (N)
  4. Water Temperature
  5. pH
  6. Nitrite-N
  7. Nitrate-N
  8. Turbidity (NTU)
  9. Biochemical Oxygen Demand (BOD ATU)
  10. Phosphorus (P)

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
   
  Below are example snapshots from the project to give a visual overview of the dashboard functionality, visual analytics, and outlier mapping:
	•	🌍 Outlier Mapping Interface
Displays pollution parameter outliers geographically across England. Users can select a pollutant (e.g., Ammonia, Turbidity) to highlight where extreme readings have occurred.
<img width="1405" alt="Screenshot 2025-04-09 at 13 12 17" src="https://github.com/user-attachments/assets/ee025f92-0cd9-47f5-8c74-2f0aced95d5f" />
<img width="1405" alt="Screenshot 2025-04-09 at 13 12 01" src="https://github.com/user-attachments/assets/37783a29-40b3-4e9a-b997-589f071a4ad3" />
<img width="1405" alt="Screenshot 2025-04-09 at 13 12 13" src="https://github.com/user-attachments/assets/cf9115c9-691b-494d-a2a6-9ca00ef5825e" />



5. **Geographic Clustering:**
   - K-Means clustering on coordinates to identify water quality zones

---

### 🛠️ Feature Engineering

- **Rolling Means:** 3/5/7-sample windows for smooth trends
- **Lag Features:** previous values to capture temporal trends
- **Water Type Averages:** used as fallback predictors
- **Outlier Frequency Scores:** how often a site showed spikes

---
## 📊 Features & Visuals

- 📌 **Dashboard** showing outliers on the map.
- 📈 **Time-series visualizations** for trends per parameter and water type.
- 🧭 **Clustering maps** to display water zones by pollution behavior.
- 📆 **Seasonal decomposition** and trends across years.

You can view them all in the `Data_Analysis_.html` and `Data_Preprocessing&ModelTraining.html` reports included in this repo.

---

## 🧪 Tech Stack

- **Language**: Python (Pandas, NumPy, Scikit-learn, LightGBM)
- **Visualization**: Matplotlib, Plotly
- **Modeling**: LightGBM (for its speed + tabular performance)
- **Platform**: JupyterLab / Streamlit (for future UI integration)

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
## 💬 Future Plans

- 🌐 **Integrate into BeAware App Frontend**  
  The trained models will be deployed into the BeAware React Native application, allowing users to view water quality predictions directly from their mobile devices.
  its an ongoign project, more details on the front end will be available on my linkedinm page soon.
  
  here is a sneek peek on whats coming.
<img width="452" alt="Screenshot 2025-04-07 at 00 09 36" src="https://github.com/user-attachments/assets/d0e7a333-6dc6-4305-bf26-7cad24b06ff0" />
<img width="452" alt="Screenshot 2025-04-07 at 00 09 49" src="https://github.com/user-attachments/assets/f76fe273-9aeb-4f89-81b8-2337cf6ca5eb" />
<img width="452" alt="Screenshot 2025-04-07 at 00 09 59" src="https://github.com/user-attachments/assets/979331e8-6d36-4e72-b27a-58ba887049b3" />
<img width="452" alt="Screenshot 2025-04-07 at 00 14 28" src="https://github.com/user-attachments/assets/9b031fc6-fe0c-49d9-8a3a-c8005ddf044c" />

- 📍 **Predict Water Quality by User Location**  
  The system will automatically determine a user’s closest water sampling site and water type, and serve them current and next-day predictions in real time.

- 📊 **Add Interactive Maps + Environmental Alerts**  
  Users will be able to explore environmental health indicators via interactive map layers, view outlier alerts, and track changes over time.

- 🧠 **Expand Scope to Other Environmental Factors**  
  Planned modules will extend this project to include:
  - Air Quality (via API)
  - Soil Fertility (sensor or vision-based)
  - Light Pollution (satellite mapping)
  - Noise Pollution (acoustic signal analysis)

---

## 🙌 Acknowledgements

This project is a solo data science and machine learning initiative by **Sofiane Belbrik**, developed as part of a broader environmental AI awareness platform called **BeAware**.

The mission is to empower users with actionable insights on environmental quality, encourage sustainable behavior, and support data-driven environmental planning through transparent and accessible tools.
