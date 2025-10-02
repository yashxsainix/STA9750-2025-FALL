# STA9750 — MP01 (Professional, subsectioned site)

This package gives you an **A‑grade**, professional site:
- Numbered sections & subsections (Intro, Acquisition, Cleaning, KPIs, EDA, Q&A, Press Releases, Conclusion)
- Rich narrative under each answer (inline R values)
- Interactive tables (DT) & clean ggplot visuals
- KPI cards & callouts
- **No Quarto** → avoids filter errors

## Build
1) Open **RStudio** in this folder.
2) Console (one‑time):
   ```r
   install.packages(c("tidyverse","DT","lubridate","stringr","rmarkdown","scales"))
   ```
3) Open **mp01.Rmd** → click **Knit** → creates **mp01.html**.
4) Publish via GitHub Pages:
   - **Root**: upload `mp01.html`, `index.html`, `about.html`, `styles.css` to repo root; Pages = `main / (root)`
   - **Docs**: upload same files into `docs/`; Pages = `main / /docs`

Site will be: `https://<username>.github.io/<repo>/mp01.html`
