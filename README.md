# Ohio Dental Clinic — Site Selection Analysis

**Which of six Ohio addresses offers the strongest combination of patient market size, demographics, competitive density, and income potential for a new general dentistry practice?**

> **Recommendation:** Lewis Center, OH (Delaware County) — composite score 81.0/100, market gap index 100.0, 2,029 residents per dentist, #1 in 4 of 5 sensitivity scenarios.

---

## Interactive Dashboard

Explore the full interactive Tableau dashboard:

[View Dashboard on Tableau Public](https://public.tableau.com/app/profile/yannick.mosunga5565/viz/OhioDentalClinicSiteSelectionAnalysis/OhioDentalClinic)

---

## Overview

A dental services organization provided six candidate addresses across Ohio for evaluation. This project builds a reproducible, data-driven site selection framework using publicly available federal data sources — mapping 10,962 licensed Ohio dental providers against Census demographics for 1,233 ZIP codes to identify which location offers the best supply-demand opportunity.

The analysis moves through four phases: market exploration, scoring model development, robustness validation, and in-depth competitive analysis. Each phase is fully scripted and reproducible.

---

## Key Findings

| Rank | Location | Score | Residents/Dentist | Gap Index | Status |
|------|----------|-------|-------------------|-----------|--------|
| #1 | Lewis Center, OH | 81.0 | 2,029 | 100.0 | ✅ Recommended |
| #2 | Sawmill Rd, Columbus | 79.4 | 1,548 | 47.4 | Strong Alternative |
| #3 | Westerville, OH | 74.2 | 1,544 | 43.3 | Viable |
| #4 | Hamilton Rd, Columbus | 69.8 | 1,913 | 71.6 | Watch List |
| #5 | Indianola Ave, Columbus | 60.0 | 1,250 | 0.0 | ❌ Not Recommended |
| #6 | Ontario, OH | 25.0 | 1,843 | 32.1 | Separate Evaluation |

**Lewis Center leads on every primary metric** — highest composite score, highest market gap index, highest residents-per-dentist ratio — and wins all four Columbus head-to-head pairwise comparisons. Rankings are identical under both min-max and z-score normalization and stable across five weight sensitivity scenarios.

---

## Project Structure

```
ohio-dental-site-selection/
├── .gitignore                        ← files excluded from GitHub
├── README.md                         ← this file
├── data/
│   └── cleaned/                      ← 7 cleaned datasets (published)
│       ├── NPI_Dentists.csv
│       ├── ohio_zip_centroids_cleaned.csv
│       ├── B01001_age_cleaned.csv
│       ├── B19013_income_cleaned.csv
│       ├── B27010_medicaid_cleaned.csv
│       ├── B15003_education_cleaned.csv
│       └── locations_master_cleaned.csv
├── prep/                             ← data preparation notebooks
│   ├── prep_01_zip_centroids.ipynb
│   ├── prep_02_income.ipynb
│   ├── prep_03_age.ipynb
│   ├── prep_04_medicaid.ipynb
│   ├── prep_05_npi_zip_fix.ipynb
│   └── prep_06_education.ipynb
├── analysis/                         ← analysis notebooks
│   ├── module1_exploration.ipynb
│   ├── module2_analysis.ipynb
│   ├── module3_analysis.ipynb
│   └── module4_analysis.ipynb
├── outputs/                          ← output CSVs
│   ├── master_dataset.csv
│   ├── competitors_summary.csv
│   ├── module2_scores.csv
│   ├── module3_sensitivity.csv
│   ├── module4_final_scores.csv
│   ├── module4_tradeoff.csv
│   ├── tableau_locations.csv
│   ├── tableau_demographics.csv
│   └── tableau_competitors.csv
├── charts/                           ← all chart PNGs
│   ├── charts_m1/                    ← 8 charts (Module 1)
│   ├── charts_m2/                    ← 7 charts (Module 2)
│   ├── charts_m3/                    ← 6 charts (Module 3)
│   └── charts_m4/                    ← 6 charts (Module 4)
└── reports/                          ← local only, not on GitHub
    ← Word, PDF, and PowerPoint deliverables
```

---

## Data Sources

| Source | Version | Records | Use |
|--------|---------|---------|-----|
| [CMS NPPES National Provider Registry](https://npiregistry.cms.hhs.gov/) | Feb 2026 | 10,962 Ohio providers | Competitor mapping by ZIP and practice type |
| [ACS 5-Year Estimates](https://data.census.gov/) | 2020–2024 | 1,233 Ohio ZCTAs | Population, income, Medicaid, education |
| [SimpleMaps US ZIP Database](https://simplemaps.com/data/us-zips) | Current | ~1,232 Ohio ZIPs | Geographic centroids for distance calculations |
| Candidate Locations | Provided | 6 addresses | Geocoded lat/lon for all distance calculations |

---

## Methodology

### Distance Radii
All competitor counts and demographic aggregations use **Haversine straight-line distance** from each candidate address to ZIP code centroids:
- **15-minute drive (~9 miles):** Primary competitive catchment — competitor counts, residents-per-dentist, DSO analysis
- **30-minute drive (~18 miles):** Population and income aggregation — full addressable market

### Scoring Model
Four dimensions, min-max normalized (0 = worst in group, 100 = best):

| Dimension | Weight | Direction |
|-----------|--------|-----------|
| Population (30-min) | 30% | Higher = better |
| Median Income | 30% | Higher = better |
| Competition (15-min) | 25% | **Lower = better** (inverted) |
| Medicaid Rate | 15% | **Lower = better** (inverted) |

### Market Gap Index
```
Gap Index = (Median Income × Population_30min) / Competitors_15min
```
Normalized 0–100. Measures income-weighted demand per competitor — how undersupplied a market is relative to both its size and revenue quality.

### DSO Classification
- **Keyword method:** Flags NPI organization names containing DSO-related terms (initial classification)
- **Brand-name method:** Cross-references five confirmed Ohio chains — Aspen Dental, Heartland Dental, Great Expressions, Dental Care Alliance, Bright Now / Smile Brands (validation)

---

## How to Reproduce

1. Download raw data from the sources listed above
2. Set `RAW_DATA_PATH` in each prep notebook to your local raw data folder
3. Run prep notebooks in order: `prep_01` through `prep_06`
4. Run analysis notebooks in order: `module1` through `module4`
5. All outputs will be saved to the `outputs/` and `charts/` folders automatically


---

## Tools & Skills

**Languages:** Python 3  
**Libraries:** pandas · numpy · scipy · matplotlib · seaborn  
**Methods:** Haversine distance · Min-max normalization · Z-score normalization · Pearson correlation · Sensitivity analysis · Market gap indexing · DSO cross-validation · Pairwise comparison matrix  
**Data:** CMS open data · U.S. Census ACS · Geographic centroid calculations  
**Outputs:** CSV exports · Tableau-ready files · 20+ charts · Word/PDF reports  

---

## Results Summary

**Lewis Center is the clear recommendation** based on five independent lines of evidence:

1. **Composite score:** 81.0 — highest of all six locations. Leads on income (tied), competition, and Medicaid score simultaneously.
2. **Market gap:** Index 100.0, 2,029 residents per dentist — most undersupplied location in the dataset.
3. **Stability:** #1 in 4 of 5 weight scenarios. Rankings identical under min-max and z-score normalization.
4. **DSO exposure:** 4.1% confirmed — below Columbus average. Limited institutional competition.
5. **Pairwise:** Wins all 4 Columbus head-to-head comparisons. No other location achieves a clean sweep.

**Sawmill Rd** (score 79.4) is the recommended secondary option — same income tier, 1.6-point gap, viable if Lewis Center is unavailable.

**Indianola Ave** is the least viable Columbus location. Its population advantage (1.65M — largest in the group) is entirely offset by 1,316 competing practices (0.0 competition score).

**Ontario** (Richland County) is excluded from the Columbus comparison — it is a structurally different market with a $106,490/year estimated revenue gap vs. the Columbus average at comparable patient volume.

---

## License

This project uses public federal data (CMS, U.S. Census Bureau) which is in the public domain. Analysis code is available under the MIT License.
