
# MP02 — Backyards Affordable for All by Yashpal Saini

Dual‑mode Quarto/R Markdown project. Renders **offline** (dummy data) or **online** (live ACS via `tidycensus` if `CENSUS_API_KEY` is set).

## Quickstart
1. Open this folder in RStudio.
2. (Optional) Offline mode (no internet/APIs):
   ```r
   Sys.setenv(MP02_OFFLINE = "1")
   ```
3. (Optional) Live mode (requires key):
   ```r
   Sys.setenv(CENSUS_API_KEY = "YOUR_KEY")
   ```
4. Render:
   ```r
   # Rmd
   rmarkdown::render("mp02_final.Rmd")
   # Quarto
   quarto::quarto_render("index.qmd")
   ```

Packages auto‑install with a safe CRAN mirror. Widgets (plotly/DT/leaflet) include PDF fallbacks.
