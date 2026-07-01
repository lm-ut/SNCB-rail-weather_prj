# Belgian Rail Punctuality vs Weather

Does weather actually affect train delays on the Belgian rail network? This project explores that question using SQL for data cleaning/analysis and (soon) Power BI for the dashboard.

**Status: work in progress.** Currently at the data validation stage, working with a single day of data to build and test the pipeline before scaling up. Power BI dashboard not built yet.

## Why this project

I'm a computational biologist / population geneticist transitioning into data analytics, building SQL and BI skills outside my original domain (biology) with a dataset that's directly relevant to the Belgian job market.

## Data sources

- **Train punctuality**: [Infrabel Open Data — Raw punctuality data (D-1)](https://opendata.infrabel.be/explore/dataset/ruwe-gegevens-van-stiptheid-d-1/) — per-stop arrival/departure times and delays for domestic trains
- **Weather**: [Open-Meteo Historical Weather API](https://open-meteo.com/en/docs/historical-weather-api) — hourly temperature and precipitation, free, no API key

## What's in this repo so far

- `rail_weather_analysis.ipynb` — SQL-first exploration: loads the raw CSVs into a SQLite database, profiles the data, and works through a couple of data quality questions that turned out to matter more than expected (see below).

## Findings so far

- Infrabel's delay values are in **seconds**, not minutes — confirmed by sanity-checking the average against what's physically plausible for a rail network, not just assumed from the docs.
- ~4.6% of rows have no arrival delay recorded. These aren't cancellations — they're a train's **origin station** (no arrival time is ever scheduled there). Verified by tracing individual train journeys stop by stop, not just by counting NULLs.

## Tech stack

- SQLite + SQL (data cleaning, profiling, aggregation)
- Python / pandas (data ingestion only — the analysis logic lives in SQL)
- Power BI (dashboard, coming next)

## Next steps

- [ ] Fix the per-train "final stop" logic with a proper window function (`ROW_NUMBER()`) instead of `MAX()`
- [ ] Collect multiple days of Infrabel data to support a real correlation analysis
- [ ] Join train delays with hourly weather data
- [ ] Build the Power BI dashboard: % delayed trains by hour vs. temperature
- [ ] Extend to precipitation/snow as a driver of cancellations, not just delays

## Reproducing this

1. Download the Infrabel and Open-Meteo CSVs (see links above)
2. Update the file paths in the notebook's data loading cell
3. Run the notebook top to bottom — it builds a local SQLite database (`treni_meteo.db`, not committed to this repo) and exports a summary CSV
