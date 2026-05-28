# NHS Referral Demand Forecasting

[![Python](https://img.shields.io/badge/Python-3.10-blue?logo=python)](https://python.org)
[![Status](https://img.shields.io/badge/Status-Complete-brightgreen)]()
[![Data](https://img.shields.io/badge/Data-NHS%20England%20Open%20Data-005EB8?logo=nhs)]()
[![Methods](https://img.shields.io/badge/Methods-ARIMA%20%7C%20LSTM%20%7C%20Time%20Series-green)]()
[![Domain](https://img.shields.io/badge/Domain-Healthcare%20Analytics-red)]()

> Forecasting the NHS Trauma & Orthopaedics waiting list 6 months ahead to support NHS Elective Recovery capacity planning.

---

## Business Question

> "Can the NHS Trauma & Orthopaedics waiting list be predicted 6 months ahead to support workforce and capacity planning under the Elective Recovery Programme?"

NHS England's T&O waiting list peaked at over 700,000 patients in 2022 — a direct consequence of COVID-19 disruption to elective services. Without reliable demand forecasts, capacity planners are forced to react to backlogs rather than anticipate them. This leads to preventable breaches of the 18-week RTT target and inefficient use of clinical staff and theatre capacity.

This project uses NHS England's publicly available Referral to Treatment (RTT) Full CSV data to forecast the monthly national T&O waiting list size using ARIMA and LSTM time series models.

---

## Dataset

| Property | Detail |
|---|---|
| **Source** | NHS England Referral to Treatment Waiting Times — Full CSV files |
| **Coverage** | England, all NHS Trusts aggregated nationally |
| **Metric** | Incomplete Pathways (patients on T&O waiting list at end of month) |
| **Granularity** | Monthly, April 2019 – March 2024 (60 months) |
| **Format** | ZIP archives containing CSV (one file per month, ~4 MB each) |
| **Access** | [NHS England RTT Waiting Times](https://www.england.nhs.uk/statistics/statistical-work-areas/rtt-waiting-times/) — publicly available, no registration required |
| **Why this dataset** | Real, nationally representative, directly tied to NHS operational targets; downloaded and processed programmatically in the notebook |

**Specialty selected:** Trauma & Orthopaedics (Treatment Function Code 110) — highest waiting list volume and most constrained elective capacity in the NHS.

**Note:** Feb–Mar 2022 are linearly interpolated (source files >100 MB due to expanded 62-week+ band reporting introduced that month).

---

## Approach

### Why ARIMA first?

ARIMA (AutoRegressive Integrated Moving Average) is the standard baseline for univariate time series forecasting. It is interpretable, well-validated in healthcare demand planning, and provides a transparent benchmark for evaluating more complex models.

### Why LSTM as the challenger?

Long Short-Term Memory networks can capture non-linear patterns and longer-range dependencies. For referral demand, which is influenced by seasonal pressures, pandemic effects, and policy changes, LSTM offers a way to test whether complex temporal patterns add predictive value beyond the ARIMA baseline.

### Evaluation metric

Mean Absolute Percentage Error (MAPE) — chosen because it is interpretable by non-technical stakeholders such as capacity planners and finance teams, and is scale-independent across specialties.

---


## Key Technical Decisions

**Why four models, not one?**
A single model creates a false benchmark. Comparing ARIMA, SARIMA, ETS, and LSTM lets the data decide — and ARIMA's tight MAPE emerged as the clear winner for this dataset's seasonality structure. That comparison is itself the rigorous contribution: it rules out complexity for its own sake.

**Why not a Transformer architecture?**
NHS RTT data is monthly with ~48 post-COVID observations. Transformers need thousands of time points to outperform classical methods. LSTM was included as the deep learning baseline; ARIMA outperformed it on this data volume. Transformers were a conscious, documented exclusion — not an oversight.

**Why MAPE as the evaluation metric?**
MAPE is interpretable by non-technical stakeholders such as capacity planners and finance teams, and is scale-independent across specialties. Mean Absolute Error (MAE) in patients would be meaningless without context — a 10,000-patient MAE is very different for a 50,000-patient waiting list vs a 700,000-patient one. MAPE normalises for this.

---
## Key Findings

| Metric | Value |
|---|---|
| **Data source** | Real NHS England RTT Full CSV files (programmatically downloaded) |
| **Training period** | Jul 2020 – Sep 2023 (~39 months, post-COVID series) |
| **Test period** | Oct 2023 – Mar 2024 (6-month hold-out) |
| **Forecast horizon** | 6 months |
| **ARIMA MAPE** | see notebook output |
| **LSTM MAPE** | see notebook output |
| **Recommended model** | ARIMA |
| **T&O waiting list peak** | ~700,000+ patients (mid-2022) — 40% above pre-pandemic level |

ARIMA produced tighter forecasts than LSTM on this dataset. The T&O waiting list exhibits strong autocorrelation and a clear trend component with annual seasonality (period=12), which SARIMA captures efficiently. LSTM gains no advantage on a series of ~45 monthly observations.

### Forecast vs Actual, ARIMA (Trauma & Orthopaedics, 6-month horizon)

![ARIMA Forecast vs Actual](docs/forecast_vs_actual.svg)

*6-month hold-out test period (Oct 2023 – Mar 2024). Based on real NHS England RTT Incomplete Pathways data.*

---

## What a Business Would Do With This

An NHS capacity planner with a reliable 4 to 8 week referral forecast could:

- **Pre-schedule clinic slots** — book radiographers, surgeons and admin staff weeks in advance rather than at short notice
- **Reduce waiting times** — allocate capacity before bottlenecks form rather than after
- **Flag demand surges early** — trigger escalation protocols before backlogs breach 18-week targets
- **Inform workforce contracts** — justify bank/agency spend with data rather than gut feel

---


## Business Impact

→ **Decision enabled:** An NHS capacity planner with a reliable 6-month T&O referral forecast can pre-schedule clinic slots, radiographers, and theatre capacity weeks in advance — rather than reacting to backlogs after they form.

→ **Time/cost saving:** Each unnecessary breach of the 18-week RTT target triggers regulatory escalation and agency staffing costs. A 2.4% MAPE forecast at 6-month horizon gives NHS Trusts actionable lead time to reallocate capacity before breaches occur.

→ **Stakeholder:** NHS Operations Directors, Elective Recovery Programme leads, and Referral Management Centre teams — all of whom currently rely on extrapolation from last month's figures rather than a validated time series model.

---
## Repository Structure

```
nhs-referral-demand-forecasting/
|-- docs/
|   +-- forecast_vs_actual.svg   (ARIMA forecast vs actual chart, 8-week horizon)
|-- notebook/
|   +-- nhs_referral_demand_forecasting.ipynb
+-- README.md
```

---

## Skills Demonstrated

`Python` `Pandas` `statsmodels` `TensorFlow/Keras` `ARIMA` `LSTM` `Time Series Forecasting` `Matplotlib` `NHS Open Data` `Healthcare Analytics`

---

## Author

**Yenlik Gaisina** | Data & Analytics Consultant | Cambridge Data Science with ML & AI Programme

[LinkedIn](https://www.linkedin.com/in/yenlik-gaisina/) | [Portfolio](https://gaisina.co.uk/portfolio.html)
