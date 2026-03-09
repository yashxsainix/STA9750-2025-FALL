# NYC Housing Equity & Policy Analytics

A spatial data analysis project investigating whether neighborhood income levels shape how New York City's housing complaint systems work — not just who complains, but how quickly and consistently those complaints get resolved.

Built entirely in R and Quarto. All analysis is reproducible and presented as web-based reports.

---

## The Central Question

NYC's 311, HPD, and DOB systems collectively receive millions of housing-related complaints per year. The standard assumption is that these administrative records are a neutral reflection of housing conditions on the ground. This project tests that assumption.

The approach is indirect but deliberate: rather than directly measuring whether the city responds differently to complaints in different neighborhoods, the project asks a simpler question first — can you predict a neighborhood's median income from its complaint patterns alone?

If yes, it means income is not just an external variable that explains housing conditions. It is encoded in the administrative data itself — in what gets reported, how often, what type, and how long it takes to resolve.

---

## Data

All data is aggregated to the census tract level for spatial consistency.

| Source | Data | Access |
|---|---|---|
| NYC 311 Service Requests | Housing complaint records, 2023 | NYC Open Data API |
| Housing Preservation and Development (HPD) | Complaints and violations | NYC Open Data API |
| Department of Buildings (DOB) | Complaints and enforcement actions | NYC Open Data API |
| American Community Survey (ACS) | Median household income, rent burden, housing units, population | tidycensus R package |

Spatial joins connect complaint data to census tracts. This is not a trivial step — complaints need to be geocoded and matched to the correct tract boundary, and tracts vary significantly in size and population density across the five boroughs.

---

## Feature Engineering

Features are engineered at the census tract level and grouped into four families:

**Structural controls**
Rent burden, total housing units, population size, borough indicators. These capture the baseline housing stress in a neighborhood before looking at complaint behavior.

**Complaint intensity**
Complaint rates per capita by source (311, HPD, DOB). A higher complaint rate per resident can signal either worse housing conditions or higher civic engagement — distinguishing between these is part of what makes the modeling interesting.

**Complaint composition**
Shares of complaints by agency and type. The proportion of heat-related complaints in winter, for example, varies significantly by neighborhood income in ways that go beyond building age or type.

**Resolution outcomes**
Mean and median resolution time per tract. Unresolved complaint rates. These are the features that turned out to be the most predictive, which is the core finding of the project.

---

## Modeling

**Outcome variable:** log(median household income) from ACS  
**Split:** 70% training, 30% test  
**Validation:** 5-fold cross-validation on the training set  
**Evaluation metrics:** RMSE, MAE, out-of-sample R²

Three models were trained and compared:

**OLS (baseline)**
Standard linear regression. Included as a benchmark and for interpretability. Coefficients directly readable as the expected change in log income per unit change in each feature.

**Elastic Net (glmnet)**
Regularized regression combining L1 and L2 penalties. Better handles multicollinearity between complaint features (many are correlated with each other and with income). Alpha and lambda tuned via cross-validation.

**Random Forest (ranger)**
1,000 trees. Hyperparameters tuned via grid search: `mtry` (number of features considered at each split), `splitrule`, and `min.node.size`. Random Forest was chosen over gradient boosting because the feature importance output is more interpretable for a policy audience, and because the goal is understanding which complaint features predict income, not maximizing predictive accuracy at the expense of explainability.

**Results:**

| Model | Out-of-Sample R² |
|---|---|
| OLS | Baseline |
| Elastic Net | Improved over OLS |
| Random Forest | 0.54 (best) |

An R² of 0.54 on held-out data means that complaint system features alone — with no direct measure of building age, landlord history, or property values — explain more than half the variation in neighborhood median income across NYC census tracts. That is a substantial result for a model built entirely from administrative complaint records.

---

## Key Findings

**Income is predictable from complaint data alone**
The Random Forest model predicted neighborhood median income at R² = 0.54 using only complaint rates, complaint composition, and resolution outcomes. This is the headline result. It means socioeconomic context is embedded in these administrative systems, not merely correlated with them from the outside.

**Resolution time widens by income quintile**
Lower-income neighborhoods wait longer for complaint resolutions across all three complaint categories analyzed: heating, water leaks, and structural issues. The gap is not marginal. The partial dependence plots show a clear monotonic relationship between income quintile and median resolution time.

**The gap is widest for unresolved complaints**
Complaints that go unresolved entirely are disproportionately concentrated in lower-income tracts. This is the finding that matters most for policy because an unresolved complaint is not just a delayed response — it is a signal that the system did not register the issue as requiring action at all.

**Rent burden and unresolved complaint rates are the strongest predictors**
Feature importance analysis from the Random Forest puts these two variables at the top. Rent burden captures structural housing stress. Unresolved complaint rate captures how the system responds to that stress. Together they account for more predictive power than any other feature family in the model.

**Water leak complaints show a distinct income effect**
The relationship between income and water leak complaint prevalence is nonlinear in a specific way: mid-income tracts have lower complaint rates than both high and low-income tracts. This suggests that high-income residents report more because they are more likely to engage with city systems, while low-income residents report more because the underlying conditions are worse. The middle range reflects a combination of fewer complaints and better resolution.

**Spatial clustering of unresolved complaints**
Hotspot maps show that unresolved complaints cluster geographically in ways that align with known low-income and high-density neighborhoods, but the clustering is not uniform within those areas. Some tracts within predominantly lower-income boroughs have markedly lower unresolved rates, which points toward block-level or building-level factors that a census tract analysis cannot fully capture.

---

## Visualizations

The Quarto reports include the following figures:

- Predicted vs observed neighborhood income (scatter with residuals)
- Most important predictors of neighborhood income (feature importance bar chart)
- 311 complaint rate per 1,000 residents by tract
- Partial dependence of complaint rate on income
- Resolution time by income quintile
- Resolution time by income quintile and complaint type
- Complaint prevalence vs income by complaint type
- Top 15 complaints across income quintiles
- Income effect on water leak complaints
- Unresolved complaint rate by income quintile
- Unresolved complaint rate over time by agency
- Predicted probability of unresolved complaints
- Monthly share of complaints by system (2023)
- Hotspot map of unresolved 311 housing complaints
- Spatial clustering of unresolved complaints

---

## Limitations

**Reporting bias** — 311 complaints are not a neutral measure of housing need. Higher-income residents and more civically engaged communities file more complaints regardless of actual conditions. This means complaint rates simultaneously reflect need and capacity to report, and separating the two requires data this project does not have.

**Cross-sectional scope** — the analysis covers 2023 only. Year-to-year changes in complaint patterns, resolution times, or agency staffing are not captured. A panel extension across multiple years would substantially strengthen any causal claims.

**Predictive, not causal** — the model predicts income from complaint features. It does not establish that low income causes slower resolution, even though the partial dependence plots suggest a strong relationship. Confounders like building age, landlord responsiveness, and legal/ownership structure are not in the model.

**Unobserved factors** — landlord litigation history, building ownership type (private vs NYCHA), and proximity to city agency offices all plausibly affect resolution times and are not included.

---

## Future Work

- Multi-year panel extension to capture trend dynamics
- Systematic error analysis identifying tracts where the model over or under-predicts income
- Interaction effects between income, rent burden, and housing stock age
- Adding Housing Court filing data for a more complete enforcement picture
- Disaggregating to sub-tract level where the data supports it

---

## Tech Stack

| Tool | Purpose |
|---|---|
| R | Data processing, modeling, visualization |
| Quarto | Reproducible web-based reporting |
| tidycensus | ACS data access |
| ranger | Random Forest implementation |
| glmnet | Elastic Net implementation |
| sf | Spatial joins and geographic analysis |
| ggplot2 | Visualization |
| NYC Open Data API | 311, HPD, DOB complaint data |

---

## How to Run

**Requirements:**
- R 4.3+
- Quarto CLI
- R packages: tidyverse, tidycensus, ranger, glmnet, sf, ggplot2, quarto

```r
# Install dependencies
install.packages(c("tidyverse", "tidycensus", "ranger", "glmnet",
                   "sf", "ggplot2", "yardstick", "vip", "pdp"))

# Set your Census API key (free at api.census.gov)
tidycensus::census_api_key("YOUR_KEY_HERE", install = TRUE)

# Render the Quarto report
quarto::quarto_render("analysis.qmd")
```

---

## References

Wang, L., Qian, C., Kats, P., Kontokosta, C., & Sobolevsky, S. (2017). Structure of 311 service requests as a signature of urban location. *PLOS ONE.*

Kontokosta, C. E., & Hong, B. (2021). Bias in smart city governance: How socio-spatial disparities in 311 complaint behavior impact fairness. *Sustainable Cities and Society.*

---

## Author

**Yash Saini**
MS in Business Analytics, Baruch College
[github.com/yashxsainix](https://github.com/yashxsainix) · [linkedin.com/in/yash-saini-analyst](https://linkedin.com/in/yash-saini-analyst)
