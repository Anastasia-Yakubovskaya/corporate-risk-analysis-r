# Legal Entity Risk Analysis & Rule-Based Scoring

Analysis of corporate risks, phonebook reputation markers, credit claim history, and public platform activity for 15,000 legal entities.

## Objective
Identify risk predictors across heterogeneous data sources (phonebook records, credit applications, court/MVD requests, and external review platforms) and implement a heuristic Rule-Based Risk Scoring framework to categorize entities into risk profiles.

## Dataset Description
The analysis covers **15,000 legal entities** across **35 variables**, combining internal scoring, registry logs, and external signals:
* **Identification & Activity:** Company ID, scoring timestamp, credit application volume (`appl_num`), MVD requests.
* **Phonebook Indicators:** Total contact counts, fraudulent/dirty entries, bad record percentages for both company (`phb_*`) and management (`phb_*_dir`).
* **Credit & Legal Data:** Credit claim volumes (`claims_num`), claim percentages (`claims_percent`), court case counts, and defendant parameters.
* **External Review Platforms:** Ratings and review volumes across Yandex, 2GIS, Avito, and Yell.

Data pre-processing involved log-transformations (`log1p`) to mitigate severe right-skewness in count features and explicit missingness handling for legal/court attributes (where `NA` reflects absence of legal filings or platform profiles).

## Key Research Findings

1. **Company Size vs. Phonebook Negative Records:**
   * Phonebook negative tags are significantly more frequent in large enterprises (24.68% of companies with >5,000 contacts contain negative tags vs. 11.22% in small entities). 
   * *Insight:* Isolated negative marks in large companies are normal operational artifacts, whereas negative tags in small entities serve as a high-risk flag.

2. **Credit Claims & Phonebook Negative Correlation:**
   * No direct correlation observed between high credit claim percentages and phonebook negative records (e.g., 17.43% risk in zero-claim vs. 16.22% in medium-claim groups). These features provide orthogonal risk signals.

3. **Public Activity Impact:**
   * Public visibility correlates directly with negative feedback presence (49.18% of highly active entities have negative records vs. 15.08%–16.39% in low/moderate activity groups). Negative tags in inactive entities signal elevated risk.

4. **Financial Activity Dynamics:**
   * Risk remains stable during initial credit application stages (1–10 applications), but escalates sharply to **33.57%** for entities submitting >10 credit applications, indicating acute liquidity stress.

5. **Rule-Based Risk Scoring:**
   * Implemented a heuristic scoring model evaluating 4 risk components: company phonebook negative share, management phonebook flags, credit claim share, and anomalous credit application volume.
   * **Distribution:** 10,476 Moderate Risk, 2,352 Low Risk, and 2,172 High Risk entities.

## Stack
* **Language & Environment:** R, RStudio
* **Data Processing & Viz:** `tidyverse` (`dplyr`, `ggplot2`), `gridExtra`, `corrplot`, `knitr`
