# Source Inventory

Tested from Databricks Free Edition (Serverless) on 2026-11-09.

| Source | Dataset | API type | Refresh frequency | Unique key candidate | Test status | Access limitations |
|---|---|---|---|---|---|---|
| IRCC | Express Entry rounds (draws) | JSON over HTTPS | Irregular, after each draw (not on a fixed schedule) | `drawNumber` | Passed: HTTP 200, 454 rounds, fields include drawNumber, drawDate, drawName, drawCRS, drawSize | None observed. All values are strings and some fields contain raw HTML |
| IRCC | Study permit holders (CKAN ID 90115b00-...) | CKAN `package_show` JSON | TODO: confirm on catalogue page (likely monthly) | Composite: year/month + province + study level (confirm from CSV) | Passed: resources listed with names, formats, URLs | First request returned HTTP 429. Fixed with User-Agent header and retry. Many resources are marked ARCHIVED |
| IRCC | Permanent residents (CKAN ID f7e5498e-...) | CKAN `package_show` JSON | TODO: confirm on catalogue page | Composite: year/month + province + category (confirm from CSV) | TODO: record result | Same 429 behaviour as above |
| IRCC | Express Entry PR admissions (CKAN ID 52e4b14b-...) | CKAN `package_show` JSON | TODO: confirm on catalogue page | Composite: year/month + province + occupation (confirm from CSV) | TODO: record result | Same 429 behaviour as above |
| Statistics Canada | Table 14-10-0444-01, job vacancies and offered hourly wages | WDS REST `getFullTableDownloadCSV` (PID 14100444) | Quarterly | Composite: REF_DATE + GEO + NOC + statistic | Passed: HTTP 200, status SUCCESS, ZIP URL returned (ZIP not downloaded) | None observed |
| Statistics Canada | Table 14-10-0417-01, annual employee wages by occupation | WDS REST `getFullTableDownloadCSV` (PID 14100417) | Annual | Composite: REF_DATE + GEO + NOC + wage type | Passed: HTTP 200, status SUCCESS, ZIP URL returned (ZIP not downloaded) | None observed |
| IRCC | Resource file downloads (www.ircc.canada.ca) | HTTPS file download | Follows parent dataset | n/a | TODO: record HEAD request result | TODO |

## Notes
- No outbound blocking was observed for canada.ca, open.canada.ca or statcan.gc.ca.
- Key candidates marked "composite" must be confirmed once the CSV columns are inspected in Phase 2.