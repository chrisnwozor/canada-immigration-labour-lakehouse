# Canada Intelligence: Immigration and Labour Market Lakehouse

A Databricks lakehouse that ingests Canadian government data (IRCC and Statistics Canada) to help international students compare PR pathways, jobs and provinces.

## Architecture (planned)
- **Sources:** IRCC Express Entry JSON, IRCC open-data CSVs (via CKAN), Statistics Canada WDS tables
- **Ingestion:** Python `requests` in Databricks notebooks (GitHub Actions as fallback)
- **Storage:** Unity Catalog `canada_intelligence` with `bronze`, `silver`, `gold`, `ops` schemas and a managed Volume `bronze.landing`
- **Layers:** Bronze (raw) → Silver (typed, cleaned) → Gold (analytics-ready)

## Sources
See [docs/source_inventory.md](docs/source_inventory.md).

## Phase 1 achievements
- Notebook runs on Serverless compute and is linked to this repo
- Express Entry JSON: HTTP 200, 454 rounds
- CKAN metadata API: works for all three IRCC datasets
- Statistics Canada WDS: SUCCESS for tables 14100444 and 14100417

## Free Edition connectivity notes
- No outbound blocking observed for canada.ca, open.canada.ca or statcan.gc.ca
- open.canada.ca returned HTTP 429 on the first attempt; a User-Agent header plus retry logic fixed it
- Express Entry values arrive as strings and some fields contain HTML; cleaning happens in Silver

## Data rules
1. IRCC aggregate data may be rounded or suppressed, so totals may not reconcile exactly.
2. Check NOC versions before joining IRCC and Statistics Canada data.
3. Admissions, job-market trends and eligibility rules are kept separate. The data cannot predict an individual's chance of PR.