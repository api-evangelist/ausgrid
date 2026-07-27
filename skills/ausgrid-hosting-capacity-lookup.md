---
name: Look up Ausgrid unallocated hosting capacity for a zone substation
description: >-
  Find how much additional load or generation the Ausgrid network can absorb at a given
  zone substation and forecast year, by querying the Ausgrid unallocated hosting
  capacity (UHC) ArcGIS REST layers. Use when siting solar, batteries, EV charging or
  new load in Sydney, the Central Coast or the Hunter.
api: arcgis/ausgrid-uhc-featureserver.json
surface: https://portal.data.nsw.gov.au/arcgis/rest/services/Hosted/Ausgrid_UHC_Data/FeatureServer
operations:
  - GET /Ausgrid_UHC_Data/FeatureServer/0/query
  - GET /Ausgrid_UHC_Data/FeatureServer/1/query
generated: '2026-07-27'
method: generated
---

# Ausgrid unallocated hosting capacity lookup

**Read this first.** Ausgrid operates no API. These endpoints are operated by the **NSW
Government** (`portal.data.nsw.gov.au`) and publish Ausgrid-owned data — every record
carries `owner: "Ausgrid"`. There is no authentication, no key and no rate-limit policy.
Do not describe this to a user as "the Ausgrid API".

## Layers

| Layer | Path | Records | Meaning |
|---|---|---|---|
| Primary | `/Ausgrid_UHC_Data/FeatureServer/0` | 2,160 | Hosting capacity at the primary voltage level |
| Secondary | `/Ausgrid_UHC_Data/FeatureServer/1` | 2,120 | Secondary voltage level, plus `n_1_nameplate_capacity` |

## Steps

1. **Confirm the field names before filtering.** They are lower-cased and truncated and
   do not match the display aliases. Read them from `arcgis/ausgrid-uhc-layer0-primary.json`,
   or live:
   ```
   GET https://portal.data.nsw.gov.au/arcgis/rest/services/Hosted/Ausgrid_UHC_Data/FeatureServer/0?f=json
   ```
2. **Query by substation and year.**
   ```
   GET https://portal.data.nsw.gov.au/arcgis/rest/services/Hosted/Ausgrid_UHC_Data/FeatureServer/0/query
       ?where=substation='Aberdeen' AND year=2025
       &outFields=*
       &returnGeometry=false
       &f=json
   ```
   A verbatim response is saved at `examples/ausgrid-uhc-primary-query-response.json`.
3. **Read the answer from two fields.**
   - `available_capacity__load__at_n_` — headroom for additional **load**, at N-1.
   - `available_capacity__generation_` — headroom for additional **generation**, at N-1.
   On the secondary layer also read `n_1_nameplate_capacity` for the substation's rating.
4. **Widen the search when the substation name is unknown.** Use a LIKE clause, or pull
   the distinct list:
   ```
   ?where=substation LIKE 'Newc%'&outFields=substation,year&returnGeometry=false&f=json
   ?where=1=1&outFields=substation&returnDistinctValues=true&returnGeometry=false&f=json
   ```
5. **Map it.** Both layers are point geometry in Web Mercator (`wkid` 102100 /
   `latestWkid` 3857). Request `f=geojson` for GeoJSON, or add `outSR=4326` for WGS84.

## Rules

- **Page every broad query.** `maxRecordCount` is 2,000. Use `resultOffset` +
  `resultRecordCount`; check `exceededTransferLimit` in the response.
- **Errors arrive with HTTP 200.** Always inspect the body for
  `{"error":{"code","message","details"}}` before treating a response as data. A wrong
  field name yields `Field name [x] does not exist.` See `errors/ausgrid-problem-types.yml`.
- **Dates are epoch milliseconds** (`start_date`, `end_date`). `extract_date` is a
  *string* on layer 0 (`"20240627"`) and an *epoch date* on layer 1 — do not assume.
- **`year` is a double**, not an integer or a string: `year=2025` works, `year='2025'` does not.
- **Never write.** Capabilities are `Query` only; this is public infrastructure data.
- **State the vintage.** Report `extract_date` and `dataset` alongside any number you
  give a user — hosting capacity is a forecast snapshot, not a live measurement, and
  connection outcomes require a formal Ausgrid connection enquiry
  (https://www.ausgrid.com.au/connections).
