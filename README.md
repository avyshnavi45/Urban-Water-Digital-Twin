#  Urban Water Digital Twin — MLOps Project

> AI-powered predictive analytics and real-time simulation for urban water supply management across Bengaluru's 198+ wards.

**Live Demo:** [kzamlg2zfzgu2eqh6jv6ss.streamlit.app](https://kzamlg2zfzgu2eqh6jv6ss.streamlit.app)

---

##  Problem Statement

Urban water distribution in Bengaluru faces critical imbalances across 198+ wards — supply shortages, inaccurate demand forecasting, and no real-time decision support leave authorities reacting to crises instead of predicting them.

##  Solution

An interactive **Digital Twin** that combines Machine Learning, discrete-event simulation, and hydraulic modelling into a single deployed web application. Decision-makers can run what-if scenarios, simulate 24-hour supply/demand cycles, model pipe-network pressure, and auto-generate optimal water redistribution plans.

---

##  Features

| Module | Description |
|---|---|
| **What-If Simulation** | Adjust supply/demand sliders and get instant ML-predicted demand gap |
| **SimPy 24-Hour Twin** | Discrete-event simulation of hour-by-hour water system behaviour |
| **WNTR Hydraulic Sim** | Pipe network pressure and flow modelling for each ward |
| **Redistribution Engine** | Greedy algorithm identifies surplus wards and diverts water to deficit wards |
| **Ward Analytics** | Monthly supply vs demand charts, seasonal imbalance breakdown |

---

##  Technology Stack

| Category | Tools |
|---|---|
| **ML Model** | Random Forest Regressor (scikit-learn) |
| **Simulation** | SimPy (discrete-event), WNTR (hydraulic network) |
| **Data** | Pandas, NumPy |
| **Visualisation** | Plotly, Streamlit |
| **Deployment** | Streamlit Community Cloud (MLOps) |
| **Explainability** | SHAP |

---

##  Dataset

- **4,800+ records** across 198+ Bengaluru wards
- Ward-level BBMP water supply and consumption data
- Features: `Ward_Number`, `Month`, `Quarter`, `Season`, `Population`, `Connections`, `Connection_Density`, `Consumption`, `Supply`, `Per_Capita_Consumption`, `Supply_Per_Connection`, `Supply_Demand_Ratio`, `Imbalance_Score`, `Demand_Gap`
- Anomaly detection: records where Consumption > mean + 2×std are flagged and excluded from training

---

##  ML Pipeline

```
Raw Data → Feature Engineering → Anomaly Detection → Train/Filter
       → RandomForestRegressor(n_estimators=100) → Demand Gap Prediction
```

**Target variable:** `Demand_Gap` (MLD) — positive = shortage, negative = surplus

**Features used for prediction:**
```python
['Ward_Number', 'Month', 'Quarter', 'Population', 'Connections',
 'Connection_Density', 'Consumption', 'Supply',
 'Per_Capita_Consumption', 'Supply_Per_Connection', 'Supply_Demand_Ratio']
```

---

##  Project Structure

```
urban-water-digital-twin/
│
├── app.py                          # Main Streamlit app (latest version)
├── water_dataset_ml_ready.csv      # ML-ready dataset
├── digital_twin_final.ipynb        # EDA + model training notebook
├── requirements.txt                # Python dependencies
└── README.md
```

---

##  Setup & Run Locally

```bash
# 1. Clone the repository
git clone https://github.com/<your-username>/urban-water-digital-twin.git
cd urban-water-digital-twin

# 2. Install dependencies
pip install -r requirements.txt

# 3. Place the dataset in the same folder as app.py
#    File: water_dataset_ml_ready.csv

# 4. Run the app
streamlit run app.py
```

---

##  Requirements

```
streamlit
pandas
numpy
scikit-learn
plotly
simpy
wntr
shap
matplotlib
seaborn
```

Install all at once:
```bash
pip install streamlit pandas numpy scikit-learn plotly simpy wntr shap matplotlib seaborn
```

---

##  Simulations Explained

### 1. What-If Engine
Uses the trained Random Forest model to instantly predict the demand gap when you adjust supply or demand sliders. Shows before/after comparison with a gauge chart.

### 2. SimPy 24-Hour Twin
Runs 24 discrete hourly steps. Each step adds random noise (±10 MLD supply, ±20 MLD demand) to simulate real-world variability. Classifies each hour as Critical / Warning / Normal.

### 3. WNTR Hydraulic Simulation
Models a simplified 3-node pipe network (reservoir → entry junction → two zone junctions) and reports average pressure (metres) and flow (MLD). Falls back to ratio-based estimates if WNTR is unavailable.

### 4. Water Redistribution Engine
- Identifies surplus wards (Supply > Demand) and deficit wards (Demand > Supply) using the latest data per ward
- Uses a greedy matching algorithm to divert surplus water to deficit wards in descending order of severity
- Reports diversion plan with coverage % per deficit ward

---

##  Screenshots

> Add screenshots of the dashboard, simulation results, and redistribution plan here.

---

##  Deployment (MLOps)

This project is deployed as a live public app on **Streamlit Community Cloud**:

- Auto-redeploys on every `git push` to `main`
- No server management required
- Publicly accessible URL

**Live URL:** [kzamlg2zfzgu2eqh6jv6ss.streamlit.app](https://kzamlg2zfzgu2eqh6jv6ss.streamlit.app)

---

##  Author

**A.Vyshnavi**
- GitHub: [@your-username](https://github.com/avyshnavi45)
- LinkedIn: [linkedin.com/in/your-profile](https://www.linkedin.com/in/vyshnavi-vyshnavi-359882360/)

---

## License

This project is open-source and available under the [MIT License](LICENSE).

