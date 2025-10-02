# Mini‑Project #01 — Netflix Top 10 (Quarto website)

This repo is set up to **avoid the `docusaurus.lua` filter error** and to **publish a working site** without needing Quarto right now.

## Two ways to use

### A) Already-working website (no Quarto needed)
- The `docs/` folder already contains prebuilt HTML (`mp01.html`, `about.html`, `index.html`).
- Push this repo to GitHub (or upload via website), then set **Settings → Pages → Source = main / Folder = /docs**.
- Your site will be live at: `https://<your-username>.github.io/STA9750-2025-FALL/mp01.html`

### B) Re-render with Quarto locally (optional)
1. Ensure `_quarto.yml` has **no** `filters:` lines.
2. Remove any `_extensions` folder if present.
3. Install R packages once in RStudio Console:
   ```r
   install.packages(c("tidyverse","DT","lubridate","stringr"))
   ```
4. Click **Render** on `mp01.qmd` (it will rebuild `docs/`).
5. Commit & push to update the live site.
