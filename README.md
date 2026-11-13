# County-Level New HIV Diagnoses, SDOH, and Residential Segregation (2013–2023)

**Author:** Sade M. Graves — PhD Student, Biomedical Data Science, School of Applied Computational Sciences, Meharry Medical College
**Course:** Population Informatics (Instructor: Dr. Lubna Pinky)
**Project type:** Reproduction and extension of Garrett C, Li Z, Xia H, Li X, Qiao S. *County-Level New HIV Diagnoses, Social Determinants of Health, and Residential Segregation by U.S. Region 2013 to 2023: A Longitudinal Analysis.* AIDS Behav. 2026. https://doi.org/10.1007/s10461-026-05166-7

## What this repo holds

This repo is the data backbone for a class project that reproduces the regional GEE analysis in the paper above, then extends it with three additional analyses: (1) formal time-series decomposition of the 2013–2023 trend (including the 2020 COVID-19 disruption/rebound), (2) short-horizon forecasting of county/regional new-diagnosis rates, and (3) K-means spatial clustering of counties into SDOH/segregation typologies, mapped geographically.

## Data sources (all public, no IRB required — matches the original study's data-use statement)

| Source | What it provides | Link |
|---|---|---|
| AIDSVu (Emory University Rollins School of Public Health) | County-level new HIV diagnoses, counts and rates per 100,000, 2013–2023 | https://aidsvu.org/resources/ and https://aidsvu.org/data-methods/data-methods-statecounty/ |
| U.S. Census Bureau, American Community Survey (ACS) 5-year estimates, via the Census Data API | County-level SDOH/demographic covariates: poverty, unemployment, uninsured rate, educational attainment, race/ethnicity composition (incl. Hispanic/Latino), total population | https://www.census.gov/programs-surveys/acs and https://api.census.gov/data.html |
| USDA Economic Research Service | 2023 Rural-Urban Continuum Codes (RUCC) — the one time-invariant covariate in the original study | https://www.ers.usda.gov/data-products/rural-urban-continuum-codes |
| U.S. Census Bureau | Residential segregation measures (Gini, Delta, Spatial Proximity Index), computed from ACS data following Massey & Denton (1988) | https://www.census.gov/topics/housing/housing-patterns/guidance/appendix-b.html |

## Files

- `data/aidsvu_county_newdx_2013_2023.csv` — raw AIDSVu county-level new-diagnosis counts and rates, 2013–2023 (35,442 rows: 3,222 counties × 11 years).
- `data/acs_sdoh_county_2013_2023.csv` — raw ACS 5-year SDOH/demographic pull for the same county-years.
- `data/aidsvu_acs_county_panel_2013_2023.csv` — **the analytic panel**: AIDSVu and ACS merged on FIPS code, 19 columns, 35,442 county-year rows. 99.7% match rate; the ~99 unmatched rows are documented real-world FIPS/geography changes (Connecticut's 2022 switch to planning regions, Alaska census-area boundary revisions, and the 2015 FIPS rename of Oglala Lakota County, SD), not merge errors.
- `eda_deck/EDA_Supplement_Step1.pptx` — exploratory data analysis slide deck (distributions, regional trends, correlation structure) built from the merged panel.

## Data cleaning notes (for methods transparency)

- AIDSVu embeds negative codes (−1, −2, −9) in suppressed, low-population cells for privacy; these were treated as missing rather than numeric values.
- A county-name field in the raw AIDSVu export contained embedded line breaks that split some rows during the initial parse; this was fixed by stripping embedded newlines before building the CSV.
- ACS variable codes for the same concept (e.g., "unemployment rate," "percent without a high school diploma") are **not stable across survey vintages** — the underlying variable code can change year to year. Each year's pull resolves the correct code by matching the Census Bureau's own variable *label* text rather than assuming a fixed code, which is what makes the 2013–2023 panel comparable over time.

## Status

Segregation indices (Gini/Delta/Spatial Proximity Index) and the 2023 RUCC rural-urban classification are the remaining planned additions to this panel (implementation path: the PySAL `segregation` Python package for the indices; a direct join on the USDA RUCC file for rurality), to be finalized before the regional GEE reproduction and the clustering extension.
