# NHS Referral Demand Forecasting

**Forecasting weekly NHS Trauma & Orthopaedics referral demand to give capacity planners a reliable lead time — achieving a 2.4% MAPE on an 11-year real NHS dataset.**

![ARIMA forecast vs actual — weekly T&O referrals](docs/forecast_vs_actual.svg)

> *Forecast vs actual on the held-out test period. Trained on real NHS England RTT data, 2013–2024.*

---

## TL;DR (for the skim-readers)

| | |
|---|---|
| **Problem** | NHS capacity planners react to referral backlogs instead of anticipating them |
| **Data** | **574 weeks** of real NHS England RTT referral data (Apr 2013 – Mar 2024) |
| **Best model** | ARIMA — **MAPE 2.4%**, **MAE ≈ 1,198 referrals/week** |
| **Runner-up** | LSTM — MAPE 4.1% (the simpler classical model won) |
| **So what** | Accurate enough to pre-book clinic, theatre and workforce capacity weeks ahead |

---

## Business Question

> *"Can weekly Trauma & Orthopaedics referral demand be forecast accurately enough to let NHS capacity planners schedule staff and theatre capacity in advance — rather than reacting to backlogs after they form?"*

NHS England's T&O waiting list peaked at over **700,000 patients in 2022**, a direct consequence of COVID-19 disruption to elective services. Without reliable demand forecasts, capacity planners react to backlogs rather than anticipate them — driving preventable breaches of the 18-week Referral-to-Treatment (RTT) target and inefficient use of clinical staff and theatre time.

This project forecasts **weekly referral volumes** for Trauma & Orthopaedics using classical time-series models, validated on real NHS England open data.

## Dataset

| Property | Detail |
|---|---|
| **Source** | NHS England Referral to Treatment (RTT) Waiting Times — Full CSV files |
| **Coverage** | England, all NHS Trusts aggregated nationally |
| **Specialty** | Trauma & Orthopaedics (Treatment Function Code 110) — highest-volume, most capacity-constrained elective specialty |
| **Granularity** | **Weekly — 574 observations (Apr 2013 → Mar 2024)** |
| **Format** | ZIP archives of monthly CSVs, downloaded and resampled to a weekly series programmatically in the notebook |
| **Access** | Publicly available, no registration required |

*Note: a small number of weeks affected by source-file reporting changes are linearly interpolated; this is flagged explicitly in the notebook.*

## Approach

**Why ARIMA first?** ARIMA is the standard, interpretable baseline for univariate time-series forecasting and is well-validated in healthcare demand planning. It gives a transparent benchmark before reaching for complexity.

**Why an LSTM challenger?** LSTMs can capture non-linear, longer-range patterns. Including one tests directly whether deep learning adds predictive value over the classical baseline on this data — rather than assuming it.

**Why compare multiple models (ARIMA, SARIMA, ETS, LSTM)?** A single model is a false benchmark. Letting the data choose is the rigorous contribution: it *rules out complexity for its own sake*. On this series, the ARIMA family won.

**Why not a Transformer?** Transformers need thousands of time points to beat classical methods; even at weekly granularity this series is far smaller. The exclusion was a conscious, documented decision — not an oversight.

**Why MAPE?** It's scale-independent and interpretable by non-technical stakeholders (capacity planners, finance). A raw MAE in referrals is meaningless without context; MAPE normalises it.

## Key Findings

| Model | MAPE | MAE | Verdict |
|---|---|---|---|
| **ARIMA** | **2.4%** | **≈ 1,198 referrals/week** | **Recommended — best accuracy, fully interpretable** |
| LSTM | 4.1% | ≈ 2,056 referrals/week | Underperformed the classical baseline at this data scale |
| SARIMA / ETS | also evaluated | — | Benchmarked in the notebook; did not beat ARIMA |

*Data: 574 weekly real NHS England RTT observations (Apr 2013 – Mar 2024). T&O waiting-list peaked at ~700,000+ patients mid-2022, ~40% above pre-pandemic level.*

The weekly T&O referral series shows strong autocorrelation, a clear trend, and annual seasonality, which the ARIMA/SARIMA family captures efficiently. The LSTM gained no advantage on a series of this size — a useful negative result that justifies shipping the simpler, more interpretable model.

## What This Means in Practice

A **2.4% MAPE** means weekly referral forecasts land, on average, within ~2.4% of actual demand — accurate enough to act on. With that lead time, an NHS capacity planner could:

- **Pre-schedule clinic and theatre capacity** weeks ahead instead of at short notice
- **Flag demand surges early**, triggering escalation before 18-week RTT breaches occur
- **Justify bank/agency workforce spend** with validated forecasts rather than last month's figures
- **Reduce waiting times** by allocating capacity before bottlenecks form

**Stakeholders:** NHS Operations Directors, Elective Recovery Programme leads, and Referral Management Centre teams.

## Repository Structure

```
nhs-referral-demand-forecasting/
├── docs/
│   └── forecast_vs_actual.svg      # ARIMA forecast vs actual (weekly)
├── notebook/
│   └── nhs_referral_demand_forecasting.ipynb
└── README.md
```

## How to Run

```bash
git clone https://github.com/yenlikgaisina/nhs-referral-demand-forecasting.git
cd nhs-referral-demand-forecasting
pip install -r requirements.txt
jupyter notebook notebook/nhs_referral_demand_forecasting.ipynb
```

*(requirements: pandas, numpy, statsmodels, tensorflow, matplotlib, jupyter)*

## Skills Demonstrated

`Python` · `Pandas` · `statsmodels` · `TensorFlow/Keras` · `ARIMA` · `SARIMA` · `LSTM` · `Time-Series Forecasting` · `Matplotlib` · `NHS Open Data` · `Healthcare Analytics`

## Limitations & Honest Notes

- Forecasts national, aggregated demand; Trust-level forecasting would need local data.
- A few interpolated weeks (source reporting changes) are flagged in the notebook.
- The LSTM was deliberately not tuned to production depth — it served as a baseline challenger, and the documented finding is that the classical model wins at this data scale.

## Author

**Yenlik Gaisina** · Data & Analytics Consultant · Cambridge Data Science with ML & AI Programme
[LinkedIn](https://www.linkedin.com/in/yenlik-gaisina/) · [Portfolio](https://gaisina.co.uk)
