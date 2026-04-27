# Port Authority of New York & New Jersey — Traffic Intelligence & Forecasting

> **BANL-6100-03 | Group 1 | Spring 2026 | University of New Haven**  
> **Professor: Dr. Pindaro Demertzoglou**

---

## 📋 Project Overview

This project is a comprehensive data analytics and machine learning study for the **Port Authority of New York and New Jersey**, analyzing bridge and tunnel traffic data across **8 facilities** from **January 2015 through May 2025** (29,445 facility-day records). The project answers five key business questions using three predictive models, an interactive Power BI dashboard, and a live web application deployed on Vercel.

**Team Members:**
- Harshita Bajaj
- Harshith Doli
- Khan Mazen Moid
- Manjil Adhikari
- Meghana Lakshminarayana Swamy

---

## 📁 Repository Structure

```
PortAuthority-NY-NJ-TrafficFlow_CapstoneProject/
│
├── README.md
├── index.html                        # Live web app (deployed on Vercel)
├── Group1_PPR3_Presentation.pptx     # Final presentation slides
├── PowerBI_Dashboard.pbix            # Interactive Power BI dashboard
│
├── data/
│   ├── Traffic_by_Facility_FINAL-1.csv     # Master dataset (29,445 rows)
│   └── forecast_original_ppr2.csv          # AutoTS forecast 2025–2032 (yearly)
│
└── reports/
    ├── PPR1_Report.docx              # Phase 1 — EDA & Dataset Selection
    ├── PPR2_Report.docx              # Phase 2 — Models & Deployment
    └── PPR3_Final_Report.docx        # Phase 3 — Final Report
```

---

## 🏗️ Facilities Analyzed

| Facility | Type |
|----------|------|
| Bayonne Bridge | Bridge |
| George Washington Bridge Lower | Bridge |
| George Washington Bridge Upper | Bridge |
| George Washington Bridge PIP | Bridge |
| Goethals Bridge | Bridge |
| Holland Tunnel | Tunnel |
| Lincoln Tunnel | Tunnel |
| Outerbridge Crossing | Bridge |

---

## ❓ Business Questions

| # | Question |
|---|----------|
| Q1 | What are the top 5 factors that affect the usage of bridges and terminals by drivers? |
| Q2 | How many toll violators are there? By year, month, and facility. |
| Q3 | What are the busiest times throughout the year and how is traffic affected by seasonality? |
| Q4 | Report on congestion pricing in 2025 — is there a shift in traffic patterns? |
| Q5 | Forecast the usage of facilities by year beyond 2025 to the maximum year possible. |

---

## 🤖 Models Built

### Model 1 — Regression (Q1, Q3, Q4)

> Predicts daily `total_vehicles` per facility

| Metric | Value |
|--------|-------|
| Algorithm | H2O AutoML — Stacked Ensemble |
| Models Evaluated | 282 |
| R² | 0.9791 |
| RMSE | 2,718 vehicles/day |
| Top Feature | Facility Identity (75.18%) |

**Top 5 Features:**

| Rank | Feature | Importance |
|------|---------|-----------|
| 1 | Facility Identity | 75.18% |
| 2 | EZPass Ratio | 13.04% |
| 3 | Gas Price | 3.31% |
| 4 | Day of Year | 2.12% |
| 5 | Year / Trend | 2.12% |


<img width="1154" height="551" alt="image" src="https://github.com/user-attachments/assets/e2f32b9b-0f02-4b03-8aca-e2f5653a5fa0" />


<img width="726" height="415" alt="image" src="https://github.com/user-attachments/assets/6f64b704-c017-4b60-9501-d2f9b1c868f5" />


---

### Model 2 — Classification (Q2)

> Predicts `high_violation` (binary) — violation rate > 6.18%

| Metric | Value |
|--------|-------|
| Algorithm | H2O AutoML — GBM Bernoulli |
| AUC | 0.9985 |
| F1 Score | 0.9711 |
| Recall | 96.79% |
| False Alarm Rate | 1.02% |

<img width="1035" height="360" alt="image" src="https://github.com/user-attachments/assets/4f7422e2-9700-4ae7-8329-f0248ed33d26" />


---

### Model 3 — Time Series Forecasting (Q5)

> Forecasts daily `total_vehicles` per facility 2025–2032

| Metric | Value |
|--------|-------|
| Algorithm | AutoTS — Horizontal Ensemble |
| Training Period | Jan 2015 – Dec 2024 |
| SMAPE (Best Window) | 5.23% |
| Forecast Horizon | 2025 – 2032 (2,922 days) |

**Forecast Summary (Millions of Vehicles):**

| Year | Bayonne | GWB Lower | GWB Upper | Goethals | Holland | Lincoln | Outerbridge |
|------|---------|-----------|-----------|----------|---------|---------|-------------|
| 2025 | 4.33 | 22.82 | 27.17 | 18.56 | 15.80 | 18.76 | 14.99 |
| 2028 | 4.69 | 23.29 | 26.50 | 19.75 | 15.94 | 18.40 | 15.15 |
| 2032 | 5.17 | 23.94 | 25.29 | 21.27 | 16.03 | 17.78 | 15.28 |

<!-- SCREENSHOT PLACEHOLDER -->
> 📸 **[INSERT SCREENSHOT: AutoTS Jupyter output — best model summary and SMAPE]**

<!-- SCREENSHOT PLACEHOLDER -->
> 📸 **[INSERT SCREENSHOT: Forecast line chart from Jupyter — all facilities 2025–2032]**

---

## 🚀 Deployment — FastAPI + Docker + Kubernetes

### Architecture

```
H2O MOJO Models → FastAPI Backend → Docker Container → Kubernetes (Minikube) → Live Endpoints
```

### API Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/` | GET | Health check |
| `/predict/regression` | POST | Predict daily vehicle count |
| `/predict/classification` | POST | Predict high violation probability |
| `/predict/timeseries` | POST | Return AutoTS facility forecast |

<!-- SCREENSHOT PLACEHOLDER -->
> 📸 **[INSERT SCREENSHOT: Terminal — `docker ps` showing container running]**

<!-- SCREENSHOT PLACEHOLDER -->
> 📸 **[INSERT SCREENSHOT: Terminal — `kubectl get pods` showing pod 1/1 Running]**

<!-- SCREENSHOT PLACEHOLDER -->
> 📸 **[INSERT SCREENSHOT: FastAPI Swagger UI at localhost:8000/docs]**

---

## 🌐 Live Web Application

**URL:** [https://port-authority-ny-nj-traffic-flow-c.vercel.app](https://port-authority-ny-nj-traffic-flow-c.vercel.app)

Built as a single-page HTML + JavaScript dashboard connected to a **Supabase** cloud database with 16 pre-computed analytics tables. Deployed on Vercel via GitHub.

### Features
- 5 interactive question pages — one per business question
- Filters for Facility, Year, and Metric Focus
- Dynamic insight strip that updates based on selections
- Model attribution on every chart
- No scrolling — everything fits on one screen

<!-- SCREENSHOT PLACEHOLDER -->
> 📸 **[INSERT SCREENSHOT: Web App — Home page]**

<!-- SCREENSHOT PLACEHOLDER -->
> 📸 **[INSERT SCREENSHOT: Web App — Q1 Feature Importance page]**

<!-- SCREENSHOT PLACEHOLDER -->
> 📸 **[INSERT SCREENSHOT: Web App — Q3 Busiest Times page]**

<!-- SCREENSHOT PLACEHOLDER -->
> 📸 **[INSERT SCREENSHOT: Web App — Q5 Forecasts page]**

---

## 📊 Power BI Dashboard

Interactive dashboard answering all 5 business questions. All datasets are embedded in the `.pbix` file — no external import needed.

### Pages

| Page | Content |
|------|---------|
| Home | Navigation hub |
| Top Factors | Feature importance charts + KPI cards |
| Toll Violations | Violations by year / facility / month |
| Busiest Times | Traffic by year / month / day / facility |
| Congestion Pricing | 2023 vs 2024 vs 2025 comparison |
| Forecasts | Facility usage 2025–2032 |
| Deployment | Live web app link + team credits |

<!-- SCREENSHOT PLACEHOLDER -->
> 📸 **[INSERT SCREENSHOT: Power BI — Home page]**

<!-- SCREENSHOT PLACEHOLDER -->
> 📸 **[INSERT SCREENSHOT: Power BI — Q3 Busiest Times page]**

<!-- SCREENSHOT PLACEHOLDER -->
> 📸 **[INSERT SCREENSHOT: Power BI — Q5 Forecasts page]**

---

## 📌 Key Findings

| Question | Finding |
|----------|---------|
| Q1 | Facility Identity (75.18%) is the single most powerful predictor of daily traffic volume |
| Q2 | GWB Lower has the highest violations (17.7M total); peak year was 2020 (12.7M) |
| Q3 | Peak season is June–August; 2020 COVID shock caused −20.5% network-wide decline |
| Q4 | GWB Upper is the only facility with negative 2024 growth (−1.46%) — congestion pricing signal |
| Q5 | Bayonne grows +19.3% by 2032; GWB Upper declines −7.0% — structural congestion pricing effect |

---

## 🔮 Future Scope

Should the Port Authority provide access to a real-time traffic API, the web application and dashboard can be updated to pull live data directly — all charts and model insights would update automatically as new data flows in, transforming this into a fully operational real-time traffic intelligence platform.

---

## 📚 Tools & Technologies

| Category | Tools |
|----------|-------|
| Data Preparation | Python (pandas), SQL, Excel |
| EDA | R, Python |
| Modeling | H2O.ai AutoML, AutoTS 1.0.1, scikit-learn |
| Deployment | FastAPI, Docker, Kubernetes (Minikube), WSL2 |
| Web App | HTML/CSS/JavaScript, Chart.js, Supabase, Vercel |
| Dashboard | Power BI Desktop |
| Version Control | GitHub |

---

*University of New Haven · BANL-6100-03 · Group 1 · Spring 2026*
