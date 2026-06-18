# Rental Market Velocity as a Leading Indicator of Apartment REIT Performance

**Author:** Herman Thinnd  
**Institution:** University of British Columbia — Sauder School of Business  
**Course:** Quantitative Finance — Alternative Data Project  
**Date:** June 2026  
**Status:** Progress report — revenue growth results complete; IBES surprise and stock return analyses ongoing

---

## Overview

This project tests whether **Apartment List's metro-level Time on Market (TOM) index** — the average number of days a rental listing remains active before being leased — contains predictive information about the financial and stock-market performance of publicly traded apartment REITs.

The core hypothesis: longer time-on-market signals weaker leasing demand in a REIT's operating markets, which should show up as weaker revenue growth ahead of the REIT's own quarterly disclosure. Because Apartment List publishes this data monthly and REIT financials are disclosed on a quarterly lag, a validated relationship would represent a real, if modest, information-timing advantage.

---

## Results Summary

Five apartment REITs are analysed: **AVB, EQR, CPT, ESS, MAA** (2019Q1–2025Q4, n=140).

A 10-day increase in the exposure-weighted TOM composite is associated with a **6–7 percentage-point decline in year-over-year revenue growth**. The result passes all eight robustness checks in both the contemporaneous and one-quarter-ahead specifications, including:

- REIT fixed effects + quarter-clustered standard errors
- Driscoll-Kraay standard errors (robust to serial and cross-sectional dependence)
- Pandemic-period exclusion (2020Q1–2021Q4)
- Leave-one-REIT-out (all five exclusions remain significant)
- **Two-way (REIT + quarter) fixed effects** — the most demanding test, which removes any variation shared by all five REITs in a given quarter. The coefficient *grows* after adding quarter FE, indicating the cross-sectional within-quarter variation in TOM is more informative per unit than the shared national time-series component.
- **PCA factor decomposition** — after removing the dominant national factor (PC1, 89.7% of variance), the REIT-specific residual retains predictive content at p = 0.023.

---

## Repository Contents

| File | Description |
|------|-------------|
| `reit_report.html` | Full formatted results report (open in browser) |
| `reit_signal_analysis.ipynb` | Jupyter notebook — complete pipeline from WRDS extraction to robustness checks |
| `reit_results.json` | Machine-readable results across all specifications and checks |
| `research_summary_v2.docx` | Word document — detailed methodology and results write-up |
| `README.md` | This file |

---

## Data Sources

| Source | Access | Contents |
|--------|--------|----------|
| [Apartment List Research](https://www.apartmentlist.com/research) | Public | Monthly metro-level Time on Market index |
| Compustat Fundamentals Quarterly (`comp.fundq`) | WRDS — UBC institutional subscription | Quarterly total revenue per REIT |
| IBES Summary Statistics (`ibes.surpsum`) | WRDS — UBC institutional subscription | Analyst surprise for EPS, revenue, FFO |
| CRSP Daily Stock File (`crsp.dsf`, `crsp.dsi`) | WRDS — UBC institutional subscription | Daily stock returns + market benchmark |

REIT portfolio metro exposure weights are hand-derived from each REIT's most recent public quarterly supplemental filing, mapping disclosed market-level revenue or NOI exposure to the Apartment List metro geography.

---

## Methodology — Key Design Decisions

**Signal construction:** For each REIT, a portfolio-exposure-weighted average TOM is computed monthly across the metros in that REIT's portfolio, then averaged to quarterly.

**CRSP join:** Daily return pulls use a date-range-restricted join (`a.date BETWEEN b.namedt AND b.nameenddt`) to prevent ticker-reuse row duplication across historical name segments.

**COVID exclusion (Check 3):** The exclusion filters on the *realised outcome quarter* (`composite_quarter + lag_quarters`), not the composite's observation quarter. This prevents lagged specifications from silently retaining pandemic-period outcomes.

**Two-factor PCA test:** A two-factor residual test (removing both PC1 and PC2) was attempted and discarded — it produced false positives on synthetic null data at N=5 REITs. This is a structural inferential limit, not a coding issue, and is documented in the notebook and report.

---

## Limitations

- **Total vs. same-store revenue:** Compustat `revtq` includes acquisitions and dispositions. The TOM signal is theoretically linked to same-store performance only. Hand-collecting same-store revenue from REIT supplementals is identified as the highest-value data improvement.
- **Small cross-sectional sample:** N=5 REITs is the binding constraint on inference.
- **MAA metro coverage:** Seven of MAA's 19 portfolio metros are absent from the Apartment List file (smaller secondary Sunbelt markets).
- **CRSP vintage:** Stock return data caps at 2024-12-31; stock-return tests have a smaller sample than the fundamentals analysis.

---

## Next Steps

1. Hand-collect same-store revenue from REIT quarterly supplementals and rerun the full eight-check suite
2. Complete IBES surprise, CAR, and quarterly return analyses in the clean notebook
3. Expand the REIT universe to include IRT and NXRT as a direct test of the Sunbelt-concentration hypothesis

---

## Generative AI Usage Disclosure

This project made substantial use of **Claude (Anthropic)**, a large language model, throughout the research and development process. Specifically, AI assistance was used for:

- **Code development and debugging** — writing and iterating on the WRDS data extraction scripts, signal construction pipeline, statistical inference functions (cluster-robust OLS, Driscoll-Kraay, PCA decomposition), and the eight-check robustness suite
- **Methodology review** — identifying potential bugs in the COVID exclusion logic (lag-aware filtering) and the CRSP ticker date-range join, and suggesting the two-way fixed effects and PCA checks
- **Output verification** — reviewing regression outputs and flagging the two-factor PCA false positive issue
- **Document preparation** — drafting and formatting the HTML report, Word document, and this README

All analytical decisions, interpretation of results, choice of REITs and metro weights, and research framing are the author's own. All outputs were reviewed and verified by the author. The use of AI assistance is consistent with the course's guidelines on computational tools.

---

## Disclaimer

This repository and all associated files are produced solely for **academic research purposes** as part of a graduate finance course project at the University of British Columbia. Nothing herein constitutes investment advice, a solicitation, an offer, or a recommendation to buy, sell, or hold any security or financial instrument.

Past statistical relationships do not guarantee future results. All findings are subject to the limitations described above, including small sample sizes, measurement error, and the use of total rather than same-store revenue.

Apartment List data is used for academic research purposes only under publicly available access terms. WRDS data is accessed through UBC's institutional subscription for academic research.

The views expressed are those of the author and do not represent the views of UBC, the Sauder School of Business, Apartment List, or any other institution or data provider. No conflicts of interest are declared. The author holds no positions in any of the securities discussed.

**This repository is private.** The contents are not intended for public distribution. If you have received access to this repository, please treat the research ideas and methodology as confidential.

---

*Last updated: June 2026*
