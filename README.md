# NST DVA Capstone 2 - Project Repository

## Project Overview

| Field | Details |
|---|---|
| **Project Title** | Flight Delay And Cancellation Analysis |
| **Sector** | Aviation |
| **Team ID** | DVA-B1-G18 |
| **Section** | B |
| **Faculty Mentor** | Satyaki Das Sir |
| **Institute** | Newton School of Technology |
| **Submission Date** | 29th April 2026 |

### Team Members

| Role | Name | GitHub Username |
|---|---|---|
| Project Lead | Preetish Ubhrani | `ubhranipreetish` |
| Data Lead | Alok Singh Tomar | `AlokSinghTomar100` |
| ETL Lead | Animesh Kumar Rai | `Animesh197` |
| Analysis Lead | Sayooj S B | `SayoojSb` |
| Visualization Lead | Anshika Seth | `anshika292005` |
| Strategy Lead | Siddhanth Sadshiv Raikar | `frenemy17` |
| PPT and Quality Lead | Preetish Ubhrani | `ubhranipreetish` |

---

## Business Problem

Airline operations managers and aviation regulators are struggling to identify the root causes of flight delays and cancellations across routes.

**Core Business Question**

Which airlines, routes, seasons and delay causes are driving the worst on time performance?

**Decision Supported**

This analysis will enable airline operations managers to prioritise which delay causes, routes and carriers require targeted intervention.

---

## Dataset

| Attribute | Details |
|---|---|
| **Source Name** | Kaggle |
| **Direct Access Link** | https://www.kaggle.com/datasets/patrickzel/flight-delay-and-cancellation-dataset-2019-2023/data?select=flights_sample_3m.csv |
| **Row Count** | 3000000 |
| **Column Count** | 32 |
| **Time Period Covered** | Jan 2019 to Dec 2023 |
| **Format** | CSV |

**Key Columns Used**

**Key Columns Used**

| Column | Type | Description | Role in Analysis |
|---|---|---|---|
| `FL_DATE` | date | Flight date | Time-series analysis |
| `AIRLINE` / `AIRLINE_CODE` | string | Carrier name and IATA code | Segmentation |
| `ORIGIN` / `DEST` | string | Airport IATA codes | Route analysis |
| `DEP_DELAY` / `ARR_DELAY` | float | Delay in minutes (positive = late) | Primary KPI |
| `CANCELLED` | int | Binary cancellation flag | Cancellation KPI |
| `CANCELLATION_CODE` | string | Reason: A=Carrier, B=Weather, C=NAS, D=Security | Cause analysis |
| `DELAY_DUE_*` (5 columns) | float | Cause-attributed delay minutes | Cause breakdown |
| `DISTANCE` | float | Flight distance in miles | Correlation analysis |

For full column definitions, see [`docs/data_dictionary.md`](docs/data_dictionary.md).

---

## KPI Framework

| KPI | Definition | Formula / Computation |
|---|---|---|
| On Time Arrival Rate (%) | Measures the percentage of flights that arrive on time | (Flights where ARR_DELAY ≤ 15 mins ÷ Total Flights) × 100 |
| Flight Cancellation Rate (%) | Tracks the proportion of scheduled flights that were cancelled | (Cancelled Flights ÷ Total Scheduled Flights) × 100 |
| Median Arrival Delay | Calculates the median delay duration for flights that arrived late | Median of ARR_DELAY for all delayed flights (ARR_DELAY > 15) |

---

## Tableau Dashboard

| Item | Details |
|---|---|
| **Dashboard URL** | https://public.tableau.com/app/profile/preetish.ubhrani/viz/Flight_Delay_Analysis_Final_Dashboard/HomePage |
| **Executive Overview** | 5 KPI cards (299,532 flights · 82.44% on-time · 17.56% delay · 41-min median delay · 2.63% cancellation); Cancellation Cause bar chart; Delay Cause donut (Late Aircraft 39.08% + Carrier 34.51% = 73.58%); Delay Rate by Year (COVID dip 9.15% in 2020 → 22.07% by 2023) |
| **Delay Analysis** | 5 cause-level KPI cards with Controllable Delay at 73.58%; severity donut charts (On Time / Moderate 15–60 min / Severe 60+ min); Departure vs Arrival Delay scatter (r = 0.93); Delay Rate by Month seasonal line chart |
| **Route & Airport Network Analysis** | 4 KPI cards (airports, unique routes, busiest route, highest delay route); US Route Network Map with arc lines sized by volume; Top 10 Most Delayed Routes bar chart (all >30% · FLL→DCA leads); Route Volume vs Delay Rate scatter (negative correlation) |
| **Main Filters** | Airline (global) · Year 2019–2023 (global) — both propagate across all three dashboards simultaneously |

---

## Key Insights

1. **Delays Are Operationally Determined, Not Structural** — Distance has near-zero correlation with arrival delay (r ≈ -0.0005); intervention must target ground operations, not route length.
2. **Departure Efficiency Compounds into Arrival Recovery** — Departure and arrival delay correlation r = 0.937; every gate-recovered minute yields a 0.94-min arrival improvement. Gate ops are the single highest-leverage control point.
3. **Summer Months Are Predictably Worse and Must Be Planned For** — Jun–Aug carries 5.86 extra delay minutes vs the rest of year (p < 0.001); seasonal staffing and buffer expansion can directly offset this.
4. **Friday Is the Riskiest Day to Operate** — Friday delays are 3.2× Tuesday delays (4.82 min vs 0.79 min), confirmed by ANOVA (F=55.94, p<0.001); experienced crews and buffer time needed on Fridays.
5. **A 13.89-Minute Performance Gap Exists Between Best and Worst Carriers** — Endeavor Air at -2.65 min vs JetBlue at 11.24 min (p<0.001); bottom-quartile carriers have a proven benchmark to close against.
6. **Controllable Causes Drive 73.6% of All Delay Minutes** — Carrier ops (42.1%) + late aircraft (31.5%) = 73.6%; Weather = only 5.2%. The delay problem is overwhelmingly under airline management control.
7. **NAS Delays Have the Highest Per-Minute Impact** — NAS coefficient = 0.90 in the regression model; each NAS minute causes 0.90 min of arrival delay — advocacy for FAA airspace improvements yields outsized returns.
8. **The Regression Model Explains 93.4% of Delay Variance** — OLS R² = 0.9342; delays are highly predictable from operational inputs, meaning targeted improvements produce reliable, proportional output gains.

---

## Recommendations

4 actionable recommendations, each directly linked to a key insight above.

| # | Insight | Recommendation | Expected Impact |
|---|---|---|---|
| 1 | Carrier performance gap is 13.89 min wide (Insight 5) | Establish peer benchmarking groups; bottom-quartile carriers (JetBlue, Frontier, Spirit) adopt practices of top-quartile (Endeavor, Hawaiian, Southwest) — focus on gate turnaround, crew scheduling, maintenance positioning | JetBlue: 11.24 → 6.24 min avg delay = 2.5M+ passenger-hours recovered; ~$18–25M annual savings per major carrier |
| 2 | Summer delays are 5.86 min higher and predictable (Insight 3) | Increase ground staffing 8–12% Jun–Aug; add 4–6% buffer to block times; front-load maintenance in Apr–May to maximise aircraft availability | Recover 1–2 min of average delay at peak; improved crew utilisation and passenger satisfaction |
| 3 | Departure delay predicts arrival delay r = 0.937 (Insight 2) | Deploy real-time gate utilisation dashboards; implement predictive pushback algorithms; target 95% of flights departing within 5 min of scheduled push-back; link to incentives | 2–3 min reduction in mean departure delay; ~$8–12M annual savings per carrier in overtime and fuel |
| 4 | Late Aircraft = 39.08%, single largest delay cause (Insight 6) | Enforce 25–30 min minimum ground buffers on back-to-back routes; prioritise high-frequency corridors (SFO→LAX, ORD→LGA) where cascade risk is highest | 20% reduction = ~256,000 delay minutes recovered annually; breaking one cascade prevents 2–3 downstream delays per aircraft rotation |

---

## Repository Structure

```text
SectionName_TeamID_ProjectName/
|
|-- README.md
|
|-- data/
|   |-- raw/                         
|   `-- processed/                   
|
|-- notebooks/
|   |-- 01_extraction.ipynb
|   |-- 02_cleaning.ipynb
|   |-- 03_eda.ipynb
|   |-- 04_statistical_analysis.ipynb
|   `-- 05_final_load_prep.ipynb
|
|-- scripts/
|   `-- etl_pipeline.py
|
|-- tableau/
|   |-- screenshots/
|   `-- dashboard_links.md
|
|-- reports/
|   |-- README.md
|   |-- project_report_template.md
|   `-- presentation_outline.md
|
|-- docs/
|   `-- data_dictionary.md
|
|-- DVA-oriented-Resume/
`-- DVA-focused-Portfolio/
```

---

## Analytical Pipeline

The project follows a structured 7 step workflow:

1. **Define** - Sector selected, problem statement scoped, mentor approval obtained.
2. **Extract** - Raw dataset sourced and committed to `data/raw/`; data dictionary drafted.
3. **Clean and Transform** - Cleaning pipeline built in `notebooks/02_cleaning.ipynb` and optionally `scripts/etl_pipeline.py`.
4. **Analyze** - EDA and statistical analysis performed in notebooks `03` and `04`.
5. **Visualize** - Interactive Tableau dashboard built and published on Tableau Public.
6. **Recommend** - 3-5 data-backed business recommendations delivered.
7. **Report** - Final project report and presentation deck completed and exported to PDF in `reports/`.

---

## Tech Stack

| Tool | Status | Purpose |
|---|---|---|
| Python + Jupyter Notebooks | Mandatory | ETL, cleaning, analysis, and KPI computation |
| Google Colab | Supported | Cloud notebook execution environment |
| Tableau Public | Mandatory | Dashboard design, publishing, and sharing |
| GitHub | Mandatory | Version control, collaboration, contribution audit |
| SQL | Optional | Initial data extraction only, if documented |

**Recommended Python libraries:** `pandas`, `numpy`, `matplotlib`, `seaborn`, `scipy`, `statsmodels`

---

## Evaluation Rubric

| Area | Marks | Focus |
|---|---|---|
| Problem Framing | 10 | Is the business question clear and well-scoped? |
| Data Quality and ETL | 15 | Is the cleaning pipeline thorough and documented? |
| Analysis Depth | 25 | Are statistical methods applied correctly with insight? |
| Dashboard and Visualization | 20 | Is the Tableau dashboard interactive and decision-relevant? |
| Business Recommendations | 20 | Are insights actionable and well-reasoned? |
| Storytelling and Clarity | 10 | Is the presentation professional and coherent? |
| **Total** | **100** | |

> Marks are awarded for analytical thinking and decision relevance, not chart quantity, visual decoration, or code length.

---

## Submission Checklist

**GitHub Repository**

- [✓] Public repository created with the correct naming convention (`SectionName_TeamID_ProjectName`)
- [✓] All notebooks committed in `.ipynb` format
- [✓] `data/raw/` contains the original, unedited dataset
- [✓] `data/processed/` contains the cleaned pipeline output
- [✓] `tableau/screenshots/` contains dashboard screenshots
- [✓] `tableau/dashboard_links.md` contains the Tableau Public URL
- [✓] `docs/data_dictionary.md` is complete
- [✓] `README.md` explains the project, dataset, and team
- [✓] All members have visible commits and pull requests

**Tableau Dashboard**

- [✓] Published on Tableau Public and accessible via public URL
- [✓] At least one interactive filter included
- [✓] Dashboard directly addresses the business problem

**Project Report**

- [✓] Final report exported as PDF into `reports/`
- [✓] Cover page, executive summary, sector context, problem statement
- [✓] Data description, cleaning methodology, KPI framework
- [✓] EDA with written insights, statistical analysis results
- [✓] Dashboard screenshots and explanation
- [✓] 8-12 key insights in decision language
- [✓] 3-5 actionable recommendations with impact estimates
- [✓] Contribution matrix matches GitHub history

**Presentation Deck**

- [✓] Final presentation exported as PDF into `reports/`
- [✓] Title slide through recommendations, impact, limitations, and next steps

**Individual Assets**

- [✓] DVA-oriented resume updated to include this capstone
- [✓] Portfolio link or project case study added

---

## Contribution Matrix

This table must match evidence in GitHub Insights, PR history, and committed files.

| Team Member | Dataset and Sourcing | ETL and Cleaning | EDA and Analysis | Statistical Analysis | Tableau Dashboard | Report Writing | PPT and Viva |
|---|---|---|---|---|---|---|---|
| Preetish Ubhrani | Support | Support | Support | Owner | Support | Support | Support |
| Alok Singh Tomar | Owner | Support | Support | Support | Support | Support | Support |
| Animesh Kumar Rai | Support | Owner | Support | Support | Support | Support | Support |
| Sayooj S B | Support | Support | Support | Owner | Support | Support | Support |
| Anshika Seth | Support | Support | Support | Support | Support | Owner | Support |
| Siddhanth S. Raikar | Support | Support | Support | Support | Support | Owner | Support |

_Declaration: We confirm that the above contribution details are accurate and verifiable through GitHub Insights, PR history, and submitted artifacts._

**Team Lead Name:** PREETISH UBHRANI

**Date:** 29th April 2026

---

## Academic Integrity

All analysis, code, and recommendations in this repository must be the original work of the team listed above. Free riding is tracked via GitHub Insights and pull request history. Any mismatch between the contribution matrix and actual commit history may result in individual grade adjustments.

---

*Newton School of Technology - Data Visualization & Analytics | Capstone 2*
