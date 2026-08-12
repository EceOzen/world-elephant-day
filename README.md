# 🐘 World Elephant Day: Threat, Movement & Climate

A one-day data exploration combining three real, independent APIs to ask: *what does the current status of African elephants actually look like, at the level of individual animals and real GPS tracks — not just headline population numbers?*

Built as a break from paper-writing on 12 August 2026 (World Elephant Day). What started as a quick population-trend chart turned into a small end-to-end pipeline: conservation status → real movement telemetry → climate reanalysis, joined on space and time.

## What's in here

- **Conservation status** — live IUCN Red List API v4 queries for both African elephant species (*Loxodonta africana*, *Loxodonta cyclotis*), including Red List category, full threat breakdown with severity scores, and range-country lists
- **Real movement data** — 2.93 million GPS fixes from 15 collared elephants (Movebank study *"African Elephants in Etosha National Park"*, Wayne Getz, PI), 2008–2014, pulled via the Movebank REST API
- **Climate cross-analysis** — ERA5 monthly reanalysis (2m temperature, precipitation) for the Etosha bounding box, matched to movement data by month to test whether rainfall predicts elephant movement

## Key findings

- Both African elephant species are formally threatened but via **different mechanisms**: *L. africana* (savanna) is Endangered, driven mainly by habitat conversion; *L. cyclotis* (forest) was upgraded to **Critically Endangered in January 2026**, driven by poaching for the ivory trade ("Rapid Declines" severity, international trade flagged)
- Movement tracks a rainfall signal with a **~2-month lag** (r = 0.39 at lag 2, vs. r = 0.25 at lag 0) — elephants respond to what rain produces (temporary water, fresh forage), not to rain itself
- Females moved faster than males at every hour of the day in this dataset, with a shared late-afternoon activity peak
- Etosha National Park sits geographically separate from the KAZA transfrontier corridor network (~500–700 km northwest) — a useful reminder to verify regional assumptions against the actual study area before building analysis around them

## Data sources & APIs

| Source | What it provides | Access |
|---|---|---|
| [IUCN Red List API v4](https://api.iucnredlist.org/) | Red List category, threats, range countries, population trend | Free token, registration required |
| [Movebank REST API](https://github.com/movebank/movebank-api-doc) | Individual-level GPS telemetry | Free account; per-study download permission required |
| [Copernicus Climate Data Store (ERA5)](https://cds.climate.copernicus.eu/) | Monthly reanalysis: 2m temperature, total precipitation | Free CDS account + API key |

## Structure

```
├── elephant_day_2026_en.ipynb      # main analysis notebook (English)
├── figures
  ├── elephant_species_infographic.png
  ├── iucn_range_maps.png
  ├── elephant_threats_sankey.html
  ├── etosha_*.png / .gif             # movement visualizations
  ├── movement_vs_rainfall*.png       # ERA5 cross-analysis
```

## Running it yourself

Requirements: `pandas`, `numpy`, `matplotlib`, `xarray`, `cdsapi`, `requests`, `geopandas`, `plotly`, `contextily`, `scipy`.

```bash
pip install pandas numpy matplotlib xarray cdsapi requests geopandas plotly contextily scipy
```

You'll need your own credentials for Movebank (username/password) and the IUCN Red List API (bearer token) — the notebook prompts for these via `getpass` rather than storing them anywhere. ERA5 access requires a `~/.cdsapirc` file with your CDS API key.

## Notes & caveats

- The Etosha study covers 2008–2014, not the current period — used here because it's the richest elephant telemetry dataset available with download permission, and ERA5 covers the same window without gaps
- A calendar heatmap of movement shows apparent decline in later years; this is very likely a **collar-dropout artifact** (individuals going offline over time), not a real behavioral trend — flagged explicitly in the analysis rather than presented as a finding
- Home-range polygon comparison (male vs. female) was inconclusive with n=15; convex hulls are sensitive to outlier excursions and the sample is small for a robust sex comparison

## Author

Ece Özen İldem — [Earth Systems Playground](https://codebeyondtheearth.substack.com) 

## License

Code: MIT. Data: subject to each source's own terms (IUCN Red List, Movebank study-specific licenses, Copernicus CDS licence).
