# Medicare SNF Utilization & Payment Analysis (2019–2023)

**Author:** Melissa Chhay  
**Date:** June 2026  
**Tools:** R, R Markdown

---

## Overview

This project analyzes Medicare Skilled Nursing Facility (SNF) utilization and payment data from the CMS Medicare Post-Acute Care (PAC) Public Use Files across five performance years (2019–2023). The analysis characterizes the national distribution of standardized SNF payments, examines geographic and dual-eligibility variation, and tracks longitudinal trends using a balanced provider panel.

---

## Repository Structure

```
snf_analysis/
├── MChhay_Coding_Exercise.Rmd   # Main analysis file (code + narrative)
├── MChhay_Coding_Exercise.html  # Rendered output
├── README.md
├── data/
│   ├── PACU_2019.csv
│   ├── PACU_2020.csv
│   ├── PACU_2021.csv
│   ├── PACU_2022.csv
│   └── PACU_2023.csv
└── outputs/
    └── snf_longitudinal_trend.png
```

---

## Data Source

**CMS Medicare Post-Acute Care Utilization Public Use File**  
Available at: [https://data.cms.gov](https://data.cms.gov)

Each file contains provider-, state-, and national-level summaries of Medicare FFS SNF utilization and payments. This analysis uses `SMRY_CTGRY == "PROVIDER"` rows only. Medicare Advantage beneficiaries are excluded from this data.

**Key variables:**

- `TOT_MDCR_STDZD_PYMT_AMT` — Total Medicare standardized payment, adjusted for geographic wage differences
- `BENE_DUAL_PCT` — Percentage of beneficiaries dually enrolled in Medicare and Medicaid
- `STATE` — Two-character state/territory abbreviation
- `PRVDR_ID` — Unique CMS provider identifier

---

## Analytic Objectives

1. Characterize the cross-sectional distribution of standardized SNF payments in 2023, including national summary statistics, state-level variation, and comparison by dual-eligible beneficiary share.
2. Assess longitudinal payment trends from 2019 to 2023 using a balanced panel of providers present in all five years.

---

## How to Reproduce

1. Download the five CMS PAC PUF CSV files and place them in `data/` using the naming convention `PACU_{year}.csv`.
2. Open `MChhay_Coding_Exercise.Rmd` in RStudio.
3. Install required packages if not already available:

```r
install.packages(c("tidyverse", "janitor", "glue", "knitr", "scales"))
```

4. Click **Knit** or run:

```r
rmarkdown::render("MChhay_Coding_Exercise.Rmd")
```

The rendered HTML and saved plot will be written to `outputs/`.

---

## Key Findings

- The 2023 national sample includes 15,000+ SNF providers. The payment distribution is right-skewed, with a small number of high-payment facilities pulling the mean above the median.
- State-level variation is substantial even after geographic wage adjustment, with a top-to-bottom ratio exceeding 2x across states.
- Providers with high dual-eligible beneficiary share (>=50%) show meaningfully different mean payments than low-dual facilities; the direction and magnitude are documented in the report.
- The balanced longitudinal panel shows a payment decline during the COVID-19 disruption period (2020–2021) followed by recovery through 2023.

---

## Limitations

- **No case-mix adjustment.** Standardized payments do not adjust for patient acuity; high-payment providers may reflect higher clinical complexity rather than higher resource use per unit of need.
- **Medicare FFS only.** Medicare Advantage beneficiaries are excluded. Growing MA penetration over the study period means the FFS SNF population became increasingly selected, complicating trend interpretation.
- **Unweighted means.** All facility-level means weight each provider equally regardless of size. Volume-weighted estimates would better represent the average beneficiary experience.
- **Balanced panel survivor bias.** Facilities that entered or exited the market between 2019 and 2023 are excluded from the longitudinal analysis, likely overstating mean payments relative to the full population.
- **Ecological dual-eligibility comparison.** The dual analysis operates at the facility level and cannot support inference about individual beneficiary payments.
