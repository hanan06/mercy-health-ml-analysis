# Mercy Health Data Analysis

Case Studies in Data Science — Individual Task 1, Part 1.3

Supporting analysis for a job application to the **Data and Support Analyst** role at **Mercy Health**
(Richmond, Melbourne — hybrid). This repo explores two publicly available health/aged-care datasets
relevant to that role and applies two machine learning algorithms — **Decision Tree** and
**k-Nearest Neighbours (kNN)** — to each.

## Repository structure

```
Mercy_Health_Data_Analysis/
├── Mercy_Health_Data_Analysis.ipynb 
├── data/
│ ├── curf_aged_care_needs_2025_extract.xlsx
│ └── aihw_falls_hospitalisations_extract.xlsx
├── figures/
│ ├── graph1_mobility_by_age.png
│ ├── graph2_falls_by_age_sex.png
│ ├── confusion_matrix.png
│ └── model_comparison.png
└── README.md
```

## Datasets

### 1. AIHW GEN Aged Care Data — *People's care needs in aged care* CURF (2025)
A confidentialised unit record file (CURF): one row per person in permanent residential aged care,
with de-identified demographics and their most recent AN-ACC mobility category.

- Source / topic page: https://www.gen-agedcaredata.gov.au/topics/people-s-care-needs-in-aged-care
- Data download page: https://www.gen-agedcaredata.gov.au/resources/access-data/2025/june/gen-data-care-needs-of-people-in-aged-care
- Full raw file: 196,190 records × 10 fields (year, state, care type, sex, 5-year age group,
  Indigenous status, preferred language, country of birth, AN-ACC classification, mobility category).
- **`data/curf_aged_care_needs_2025_extract.xlsx`** is the raw file filtered to permanent residential
  care records with a known mobility category (195,725 rows) — this is the population the notebook models.

### 2. AIHW Injury in Australia (INJCAT 213) — Falls supplementary data tables
Published aggregate injury statistics from the AIHW National Hospital Morbidity Database.

- Source: https://www.aihw.gov.au/reports/injury/injury-in-australia/data
- Full raw file: 195,521 rows across six injury categories (assault, falls, object, poisoning,
  transport, and a general summary), each with multiple report tables.
- **`data/aihw_falls_hospitalisations_extract.xlsx`** is extracted from *Falls Table 8*
  ("Injury hospitalisations due to falls, by 5-year age group, sex and type of fall, Australia,
  2015–16 to 2024–25"), filtered to the `Hospitalisations` measure with `Persons`/`All ages`
  aggregate rows removed. The resulting extract contains 5,622 rows representing
  FallType × AgeGroup × Sex × FinancialYear combinations.
- The full 5,622-row extract is used in the regression analysis. Financial year is used to create
  a time-based training/test split rather than as a model predictor.


## Why these two datasets for this role?

Mercy Health runs both aged care homes and hospitals, and the Data & Support Analyst role explicitly
covers reporting, workflows and automation for data-driven decision-making. Dataset 1 profiles the
*care needs* of people already in residential aged care (relevant to staffing/equipment planning).
Dataset 2 profiles *fall-related hospital injury* in the broader population, a leading cause of hospital
admission in older Australians. Together they let an analyst reason across both the aged-care and
hospital sides of the organisation using the same style of demographic breakdown.

## Methods summary

| Dataset         | Task           | Target              | Models                        | Metrics          | Validation       |
| --------------- | -------------- | ------------------- | ----------------------------- | ---------------- | ---------------- |
| CURF care needs | Classification | `MOBILITY_CATEGORY` | Decision Tree (balanced), kNN | Macro F1, recall | Stratified split |
| AIHW falls      | Regression     | `Hospitalisations`  | Decision Tree, kNN            | MAE, R²          | Time-based split |

**Important scope note:** both source datasets report counts, not population-adjusted rates. Results
are described throughout in terms of counts/concentration ("accounts for more hospitalisations"),
not "risk" — a rate claim would need a population denominator this data doesn't provide. The two
datasets are treated as **complementary** evidence about ageing, mobility and falls, not as
validating/corroborating one another, since they measure different populations (aged care residents
vs. the wider hospitalised population).

The full written analysis — insight discussion, evaluation-metric justification, and whether the two
datasets' findings are complementary or contradictory — is submitted as part of the Part 1.3 report
(PDF), not reproduced in full in the notebook. The notebook contains brief inline notes alongside the
code explaining each modelling decision; see the report for the complete discussion and citations.

## Reproducing the analysis

```bash
pip install pandas numpy matplotlib seaborn scikit-learn openpyxl
jupyter notebook Mercy_Health_Data_Analysis.ipynb
```

## Author

Hanan Khan (s4193394) — RMIT University, COSC2669/COSC2816 Case Studies in Data Science