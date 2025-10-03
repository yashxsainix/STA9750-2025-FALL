# MP01 — Netflix Top 10 (Quarto Project)

**Course:** STA 9750 (Fall 2025)  
**Author:** Yash Saini  
**Live report:**  
- https://yashxsainix.github.io/STA9750-2025-FALL/mp01.html  
- https://yashxsainix.github.io/STA9750-2025-FALL-1/mp01.html  
*(Keep the one that matches your repo name and remove the other.)*

This repository contains my **Quarto** source (`mp01.qmd`) and the rendered HTML (`docs/mp01.html`) for **Mini‑Project #01: Gourmet Cheeseburgers Across the Globe**. The analysis downloads Netflix’s public Top‑10 datasets (global + per‑country), cleans minor issues (notably converting the literal string **"N/A"** to proper `NA`), performs EDA with **tidyverse**, **DT** tables, and **ggplot2**, and wraps findings as **press‑release style** summaries suitable for PR.

---

## Repository layout

```
.
├─ mp01.qmd          # Quarto source (the file to grade)
├─ styles.css        # Styling referenced by the .qmd
├─ _quarto.yml       # (optional) Minimal Quarto config (may be omitted)
├─ README.md         # You are here
└─ docs/             # GitHub Pages site root (served at /)
   ├─ mp01.html      # Rendered report (web‑accessible)
   ├─ styles.css
   └─ index.html     # (optional) redirects to mp01.html
```

> **GitHub Pages:** Settings → Pages → *Deploy from a branch* → **Branch:** `main` / **Folder:** `/docs`.

---

## How to render the Quarto file

> You only need **one** of these options. Option **B** is the safest if your machine inherits project filters and shows an error about `docusaurus.lua`.

### A) Standard render (typical)
```bash
quarto render mp01.qmd
```
This writes `mp01.html` to the current folder. Move or render into `docs/` before publishing (see Option C).

### B) Render with **no project filters** (avoids `docusaurus.lua`)
```bash
quarto render mp01.qmd --no-project -M filters:[] --no-cache
```
This hard‑overrides any inherited filters/extensions from parent folders and produces a clean `mp01.html` you can publish.

### C) Render straight into `docs/` (auto‑publish layout)
Put this minimal `_quarto.yml` at the repo root:
```yaml
project:
  type: default
  output-dir: docs

format:
  html:
    theme: flatly
    toc: true
    toc-depth: 3
    number-sections: true
    code-fold: true
```
Then run:
```bash
quarto render mp01.qmd
```
The output will be `docs/mp01.html` (ready for GitHub Pages).

---

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
- **scales** (axis labels; uses `label_number(scale_cut = cut_short_scale())`)

---

## Reproducibility appendix (optional but recommended)

To print environment info in the HTML, the bottom of `mp01.qmd` includes (or you may add):
```markdown
# Appendix — Reproducibility

```{r}
try(cat("Quarto: ", system("quarto --version", intern = TRUE), "\n"), silent = TRUE)
sessionInfo()
```
```
This helps graders verify your Quarto + R setup.

---

## Troubleshooting

- **GitHub Pages shows 404** → Ensure files are in **`docs/`** (not repo root), and Pages is set to **Branch `main` / Folder `/docs`**. Make a small commit to retrigger the deploy.
- **Filter error: `docusaurus.lua`** → Use the **no‑project** render:
  ```bash
  quarto render mp01.qmd --no-project -M filters:[] --no-cache
  ```
- **Theme YAML errors (e.g., `version` / `bootswatch`)** → In Quarto use:
  ```yaml
  format:
    html:
      theme: flatly
  ```
  *(Do not use the R Markdown `theme: { version: 5, bootswatch: flatly }` map.)*
- **`label_number_si()` defunct** → The code already uses:
  ```r
  scale_y_continuous(labels = scales::label_number(scale_cut = scales::cut_short_scale()))
  ```

---

## Academic integrity

This repo contains both the **Quarto source** (`mp01.qmd`) and the **rendered output** (`docs/mp01.html`) so graders can inspect code and reproduce results exactly.

---

## License

Content © 2025 Yash Saini. Datasets from Netflix Tudum Top‑10 (public). Styling adapted for educational use.
