# README

## Overview

This repository contains the replication code and data structure for "Geographic Luck and Institutional Talent: FDI, Governance, and Provincial Income Divergence in Vietnam." The analysis decomposes provincial income divergence into a geographic luck channel (FDI inflows) and an institutional talent channel (Provincial Competitiveness Index), using a balanced panel of all 63 Vietnamese provinces from 2019 to 2024.

## Variable Construction

**Dependent variable.** Real GRDP per capita, computed as nominal GRDP per capita divided by the Spatial Cost of Living Index (SCOLI) and multiplied by 100. SCOLI sets Hanoi at 100 and adjusts for interprovincial price level differences. The natural log of this measure, ln(GRDPpc_adj), is the primary outcome. Missing SCOLI values are filled first with the regional median, then the national median, then 100 if neither is available.

**Geographic luck (FDI_ihs).** Registered FDI capital in millions of USD, transformed using the inverse hyperbolic sine function. This transformation is defined at zero and for negative values, which occur due to capital repatriation accounting adjustments, while preserving an approximate log interpretation for large positive values.

**Institutional talent (PCI).** The one-period lagged Provincial Competitiveness Index, sourced from VCCI-USAID annual reports. The lag addresses reverse causality between income and governance perception.

**Geographic control (Geo-Luck).** An equal-weighted composite of port access, a binary coastal indicator, and a binary land border indicator, each scaled 0 to 100. A first-principal-component version is constructed as a robustness check.

**Instruments.** The leave-one-out (LOO) instrument is the IHS-transformed mean FDI of same-region peer provinces, excluding the province itself. A placebo instrument uses the IHS-transformed mean FDI of provinces outside the own region, used to test the exclusion restriction.

**Controls.** Trained labor share, unemployment rate, net migration rate, log population density, log registered enterprises, and log state investment (provincial if available, otherwise national, in which case it is excluded from panel estimators because it is collinear with year fixed effects).

## Key Findings

FDI and PCI each independently explain a substantial share of cross-provincial income variation, with both effects concentrated in the between-province dimension rather than within-province annual movement. The geographic composite shows no significant association with PCI, consistent with governance being a matter of administrative choice rather than geographic inheritance. The pooled coastal times PCI interaction, the direct test of whether the return to governance differs by geography, is not statistically significant, indicating a uniform return to governance reform across coastal and inland provinces. The leave-one-out instrument's exclusion restriction is supported by a weak placebo first stage and stable coefficients under year times region fixed effects saturation.

## How to Run

1. Open `Reproducible Code.py` in Google Colab.
2. Run the import cell first. The script expects the following packages: pandas, numpy, matplotlib, scipy, statsmodels, scikit-learn, linearmodels. Install linearmodels if not already available (`pip install linearmodels`).
3. Run the script. It will prompt a file upload dialog through `google.colab.files.upload()`.
4. Upload the following Excel files when prompted, matching the keys used in `FILE_CONFIG`: population and area, enterprise, FDI, geographic advantage, GRDP per capita, labor productivity, PCI score, literacy rate, trained labor share, unemployment rate, out-migration rate, in-migration rate, SCOLI, and state investment.
5. The script auto-detects column layout for SCOLI and state investment files (province by year, year by province, or cross-section) and will print a fallback warning if a file cannot be parsed.
6. All regression tables print to console in sequence: descriptive statistics, sequential OLS, IV/2SLS, variance decomposition, Anderson-Hsiao, Mundlak CRE, spatial heterogeneity (split sample and pooled interaction), LOO exclusion diagnostics, robustness checks, and overall diagnostics.
7. Two files are exported and downloaded automatically: `replication_tables.xlsx` (all tables across separate sheets) and `Figure1.png` (eight-panel summary figure).

## Notes

The panel begins in 2019 because the one-period PCI lag requires 2018 data, which is used only to construct the lag and does not appear as an estimation year itself. If the SCOLI-adjusted sample falls below 200 observations after merging, the script automatically falls back to the nominal dependent variable and prints a warning.
