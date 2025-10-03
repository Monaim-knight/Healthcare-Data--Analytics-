#  Healthcare Data Analysis – README
## Overview
This repository contains an end-to-end analysis of international healthcare facilities using a single CSV dataset (Healthcare_Data.csv). The project focuses on:

Cleaning and standardizing survey-style data
Country-level comparisons and KPIs
Predictive modeling (linear regression) for key outcomes
Comparing imputation strategies for missing data
Exportable tables and visuals suitable for presentations
The work was conducted with R in a reproducible, stepwise workflow.

Dataset
File: Healthcare_Data.csv
Example columns:
Facility_Name
Location (city)
Type (e.g., Hospital, SpecialtyCare)
Total_Beds
Rating
Date
Accreditation_Status
Country
Annual_Visits
Funding_Received_(Millions)
Note: During processing we use clean, snake_case names (e.g., funding_received_millions).

Goals
Create tidy, analysis-ready data
Summarize KPIs by country and facility type
Visualize visits, ratings, and capacity
Build interpretable regressions predicting:
Rating
Annual_Visits
Evaluate how results change under different imputation methods:
Median/Mode imputation
kNN imputation
MICE (Multiple Imputation with Chained Equations)
Methods
1) Data Cleaning
Standardized column names (snake_case)
Trimmed whitespace and harmonized categories (e.g., country, type)
Converted dates to proper date types
Ensured numeric fields are numeric
Handled single-level factors by excluding them from models to avoid contrast errors
2) Missing Data Handling
We compared three approaches:

Median/Mode: numeric columns filled with median; categorical with mode
kNN: k = 5 neighbors (VIM package)
MICE: m = 5 imputations, 10 iterations, predictive mean matching
3) Modeling
Two core linear models were fit:

Rating ~ total_beds + funding_received_millions + type + country
Annual_Visits ~ total_beds + funding_received_millions + type + country
For MICE, we pooled estimates across imputations. For the other methods we report standard model fit statistics.

4) Visualizations
Country-level bar/lollipop charts:
Average rating by country
Total/average annual visits by country
Distributions and simple comparisons where relevant
5) Exports
We export compact CSVs of:

Model coefficients and fit stats for each imputation method
Country-level KPI summaries
A model-comparison summary across imputation strategies
These are intended to drop into Excel/PowerPoint.

Repository Structure
data/
Healthcare_Data.csv
scripts/
01_cleaning.R
02_imputation_compare.R
03_modeling_and_exports.R
04_visualizations.R
outputs/
kpis_country.csv
rating_simple_coeffs.csv, rating_simple_glance.csv
visits_simple_coeffs.csv, visits_simple_glance.csv
rating_knn_coeffs.csv, rating_knn_glance.csv
visits_knn_coeffs.csv, visits_knn_glance.csv
rating_mice_coeffs.csv, visits_mice_coeffs.csv
mice_glance_approx.csv
model_comparison_summary.csv
plots/ (PNG files of charts)
Note: Filenames above reflect the intended outputs after running the full pipeline.

How to Reproduce
Requirements
R >= 4.1
Recommended packages:
dplyr, tidyr, ggplot2, broom, janitor
VIM (for kNN imputation)
mice (for multiple imputation)
readr (optional)
Install in R:
install.packages(c("dplyr","tidyr","ggplot2","broom","janitor","VIM","mice","readr"))

Run the pipeline
Set working directory to the repo root.
Execute scripts in order:
scripts/01_cleaning.R
scripts/02_imputation_compare.R
scripts/03_modeling_and_exports.R
scripts/04_visualizations.R
Outputs
CSVs will be written to outputs/
Charts will be saved to outputs/plots/
Key Findings (at a glance)
Cleaning resolved naming inconsistencies (e.g., funding column) and stabilized factor levels.
Some categorical predictors occasionally had a single observed level in subsets; these were omitted from models to avoid contrast errors.
Imputation method can influence coefficient magnitudes and model fit:
Median/Mode is simplest and stable for small gaps.
kNN may preserve local structure when data are moderately missing.
MICE provides pooled inference and typically more robust standard errors when missingness is substantive.
Refer to outputs/model_comparison_summary.csv for a concise comparison of model performance metrics across imputation strategies.

Notes and Caveats
If a factor has only one level (post-filtering), it is automatically excluded from the model formula.
Ensure column names match cleaned names if you adapt the scripts.
MICE pooling returns estimated coefficients and approximate fit summaries; pooled R-squared is summarized by averaging adjusted R-squared across imputations as a practical heuristic.
Contributing
Fork and open a PR for improvements or additional analyses.
Please include a brief description and example output screenshots/CSVs for new features.
License
MIT License (or update to your preferred license)
