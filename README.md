# fuelwatch.gr

Daily fuel prices in Greece, compared against Brent crude, refinery (ΔΕΠ) prices and the EU. A
[polasreport.gr][polasreport] production.

Static [Jekyll][jekyll] site on GitHub Pages. The page reads its data from S3 and falls back to the copy
in `assets/data/`.

## Run locally

```bash
bundle install          # once
npm ci                  # once, only needed for the data scripts
bundle exec jekyll serve -l
```

Open <http://localhost:4000>. On `localhost` the page loads `assets/data/fuel-chart.json` first and S3
second. `_config.yml` changes need a server restart.

Docker alternative: `docker compose up --build`.

## Data pipeline

Vanilla Node ESM scripts in `_scripts/`; the only npm dependency is `node-xlsx` (EU bulletin).

| Command | Does |
|---|---|
| `npm run fuel:daily` | Today's PDF, Brent, EUR/USD, EU, ΔΕΠ, then JSON |
| `npm run fuel:update` | Full rebuild from local PDFs in `_fuel-pdfs/pdfs/` |
| `npm run fuel:json` | Regenerate `assets/data/fuel-chart.json` and public CSVs |

Sources:

- Pump prices: [fuelprices.gr][fuelprices] daily PDF bulletins (needs `pdftotext` from poppler)
- Brent crude and EUR/USD: [FRED][fred]
- Refinery prices (ΔΕΠ): [oil.gge.gov.gr][gge] via [akosiaris/greek-oil-refinery-prices][akosiaris]
- EU prices: [Weekly Oil Bulletin][wob]

`_fuel-pdfs/` holds the accumulated CSV state; `assets/data/` is the generated, published copy.
`_fuel-pdfs/` and `_scripts/` are excluded from the Jekyll build.

## Deployment

- `.github/workflows/jekyll-build.yml`: builds and deploys to GitHub Pages on push to `main`.
- Daily data update: runs from the `polasreport.gr` repo (`fuel-daily-update.yml`, 07:00 UTC), which
  holds the AWS secrets. It fetches new data and uploads JSON and CSVs to the S3 bucket that
  `data_url` in `_config.yml` points to. The live page picks up new data from S3 without a redeploy.
- The scripts here are the same pipeline, for local runs and refreshing the `assets/data/` fallback.

GitHub Pages source must be **GitHub Actions** (Settings → Pages), not "Deploy from a branch".

## License

Code: MIT. Data: CC BY 4.0.

[polasreport]: https://polasreport.gr
[jekyll]: https://jekyllrb.com
[fuelprices]: https://www.fuelprices.gr/deltia_d.view
[fred]: https://fred.stlouisfed.org
[gge]: https://oil.gge.gov.gr
[akosiaris]: https://github.com/akosiaris/greek-oil-refinery-prices
[wob]: https://energy.ec.europa.eu/data-and-analysis/weekly-oil-bulletin_en
