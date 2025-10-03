# MP01 — Netflix Top 10 
**Course:** STA 9750 (Fall 2025)  
**Author:** Yashpal Saini  

**Live report:**  
- https://yashxsainix.github.io/STA9750-2025-FALL/mp01.html 

This repository contains my **Quarto** source (`mp01.qmd`) and the rendered HTML (`docs/mp01.html`) for **Mini‑Project #01: Gourmet Cheeseburgers Across the Globe**. The analysis downloads Netflix’s public Top‑10 datasets (global + per‑country), cleans minor issues (converting the literal string **"N/A"** to proper `NA`), performs EDA with **tidyverse**, **DT** tables, and **ggplot2**, and wraps findings as **press‑release‑style** summaries suitable for PR.

## How to render the Quarto file
```bash
# Standard render
quarto render mp01.qmd

```

## R packages used
Install (once) from an R console:
```r
install.packages(c("tidyverse","DT","lubridate","stringr","scales"))
```

Loaded in the analysis:
- **tidyverse** (dplyr, readr, ggplot2, etc.)
- **DT** (interactive tables)
- **lubridate** (dates)
- **stringr** (string helpers)
- **scales** (axis labels; uses)
