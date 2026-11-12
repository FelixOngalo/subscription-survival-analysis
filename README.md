# Subscription Churn: A Survival Analysis

An end-to-end survival analysis of a subscription business in R: data cleaning,
Kaplan-Meier retention curves, log-rank tests, a Cox proportional-hazards model
with diagnostics, and a customer-value and acquisition-channel analysis. The
data are simulated from a model with known coefficients, so every result is
checked against the truth.

## Question

How long do customers stay, what drives cancellation, and what is a customer
worth by plan and acquisition channel, when many customers are still active and
their final tenure is unknown (right-censoring)?

## Key findings

*(Fill in after running: 12-month retention overall and by plan, the hazard
ratios for plan, channel and usage, the value gap between referral and paid-ad
customers, and how much naive churn percentages would have been off.)*

## What is in it

- **Data cleaning** of a messy extract (duplicates, inconsistent labels,
  impossible dates, missing values), with a log of every fix
- **Why naive metrics fail:** a comparison of naive churn rates and tenure averages
  with Kaplan-Meier and the truth, including the recent-cohort trap
- **Kaplan-Meier curves**, log-rank tests, median tenure, restricted mean
  survival time, cohort retention table
- **Cox model:** hazard ratios graded against the true values, concordance,
  proportional-hazards tests
- **A deliberate violation** of proportional hazards, detected with `cox.zph`
  and examined by splitting follow-up
- **A 100-run simulation study** of the Cox model's bias and confidence-interval coverage
- **Business impact:** customer value by plan, predicted retention for example
  customers, and a standardised what-if comparison of acquisition channels

## Tools

R, survival, dplyr, tidyr, tibble, broom, readr, ggplot2, Quarto.

## Reproduce

1. `install.packages(c("survival", "dplyr", "tidyr", "tibble", "broom", "readr", "ggplot2", "scales", "knitr", "quarto"))`
2. Run in order:
   - `R/00_simulate_data.R` writes the raw CSVs and the hidden truth
   - `R/01_clean_data.R` cleans the extract and builds tenure and event
   - `R/02_kaplan_meier.R` retention curves, tests, cohorts
   - `R/03_cox_model.R` Cox model, diagnostics, simulation study
   - `R/04_business_impact.R` value and scenario analysis
3. Render `reports/report.qmd`

## Project structure

```
R/survival_functions.R     simulator, cleaning, Kaplan-Meier and Cox helpers, true survival function
R/00_simulate_data.R       raw data with quality problems, plus hidden truth
R/01_clean_data.R          cleaning and survival variables
R/02_kaplan_meier.R        retention analysis
R/03_cox_model.R           hazard ratios, diagnostics, reliability study
R/04_business_impact.R     customer value and channel scenarios
reports/report.qmd         written analysis and charts
data/raw/                  simulated raw extracts (generated)
data/clean/                cleaned data and saved results (generated)
```

## Limitations

- Simulated, well-specified data; real customers have time-varying behaviour,
  seasonality and price changes
- Channel comparisons are associations from a model, not causal effects
- Censoring is assumed non-informative (only the extraction date)
- Customer value is limited to a 24-month horizon, since the oldest customers
  have about 30 months of history
- Missing logins are imputed with the median, a simple choice
- Plan is treated as fixed at signup; upgrades, downgrades and pauses are ignored
