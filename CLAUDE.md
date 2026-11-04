# fuelwatch.gr

Standalone Jekyll site for Greek fuel prices, split out of the `/fuel` section of the sibling
`polasreport.gr` repo. Site copy is Greek; the "A polasreport.gr production" tagline stays English.

- Single page: `index.html` (markup) + `assets/js/pages/fuel-charts.js` (uPlot charts, ES modules).
- Styles: `_sass/` (BEM, variables in `_variables.scss`), entry `assets/css/main.scss`.
- Data: pipeline scripts in `_scripts/` write `_fuel-pdfs/*.csv`, then `generate_fuel_json.mjs`
  writes `assets/data/`. Never hand-edit generated CSV/JSON.
- Remote data base URL lives in one place: `data_url` in `_config.yml` (rendered into a
  `<meta name="fuel-data-url">` tag). Keep it in sync with `DATA_BUCKET` in the daily workflow.
- Local dev: `bundle exec jekyll serve -l` → <http://localhost:4000>. Do not edit `_site/`.
