# Mini‑Project #01 — Netflix Top 10 (Quarto website)

## Quick start

1. **Install Quarto**: https://quarto.org/docs/get-started/
2. **Create folder** and copy these files.
3. **Open `mp01.qmd` in RStudio** (or VS Code) and click **Render**.
4. Data auto‑downloads into `data/mp01/` and is ignored by git.
5. **Publish**: `quarto publish gh-pages` or push to GitHub and enable Pages.

## Git basics

```bash
# Replace with your info
git init
git config user.name "Your Name"
git config user.email "you@example.com"

git add .
git commit -m "MP01 scaffold"
# create repo on GitHub, then:
git branch -M main
git remote add origin https://github.com/<you>/STA9750-2025-FALL.git
git push -u origin main
```

If you see a filter error (like `docusaurus.lua`), your YAML likely references a filter from a different project. This scaffold uses plain HTML; ensure `_quarto.yml` matches and try again.
