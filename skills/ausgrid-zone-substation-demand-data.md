---
name: Get Ausgrid zone substation interval demand data
description: >-
  Retrieve Ausgrid's open 15-minute interval demand history (2005-2025, 180+ zone
  substations) and the DTAPR network layers, and correctly explain why there is no API
  for a customer's own meter data. Use for load-profile analysis, network research and
  grid modelling in Sydney, the Central Coast and the Hunter.
api: null
surface: https://www.ausgrid.com.au/about-us/about-ausgrid/research-data-sets
operations: []
generated: '2026-07-27'
method: generated
---

# Ausgrid zone substation demand data

**There is no API.** Ausgrid publishes this data as bulk files on a web page. Anything
that claims an Ausgrid REST endpoint for demand data is wrong.

## Steps

1. **Open the source page** —
   `https://www.ausgrid.com.au/about-us/about-ausgrid/research-data-sets/distribution-zone-substation-data`
   (HTTP 200). It renders one download tile per financial year, 2005 through 2025, with
   the file size inline. The FY2025 file was last updated 2025-12-09.
2. **Download the ZIP for the years you need.** Tiles resolve to opaque Sitecore Content
   Hub URLs of the form
   `https://aopt-p-001.sitecorecontenthub.cloud/api/public/content/<guid>` — anonymous
   HTTPS GET, no key, no filename in the path, no checksum. **Re-read the page to get
   current GUIDs; do not cache them as if they were an API.**
3. **Unpack.** Each ZIP contains one CSV per zone substation with 15-minute interval
   demand in MW. Ausgrid publishes no schema or data dictionary for these files —
   inspect the header row rather than assuming column names.
4. **For network topology and forecasts**, use the ArcGIS layers instead of the files:
   `https://portal.data.nsw.gov.au/arcgis/rest/services/Hosted/Ausgrid_DTAPR_2023/FeatureServer/0/query?where=1=1&outFields=object,name,voltage,dual_function&returnGeometry=false&f=json`
   — 8,696 feeder and sub-transmission segments. A verbatim response is at
   `examples/ausgrid-dtapr-feeder-query-response.json`. These endpoints are operated by
   the NSW Government, not Ausgrid. Page them: `maxRecordCount` is 2,000.
5. **For the human planning report**, use `https://dtapr.ausgrid.com.au/` (feeder
   forecasts, 11kV feeder capacity, generation hosting capacity). It is a web
   application with no API; it serves flat files from `./ausgrid_data/`.

## Rules

- **Do not use the NSW CKAN resource links.** All three Ausgrid records at
  `https://data.nsw.gov.au/data/organization/ausgrid` still point at the retired
  `/Industry/Our-Research/Data-to-share/` paths, which return 404 (re-verified
  2026-07-27). Use the `/about-us/about-ausgrid/research-data-sets/` paths.
- **Flag the licence conflict.** The NSW catalogue records these datasets as CC-BY;
  Ausgrid's own page asserts that the data "is the sole property of Ausgrid, and Ausgrid
  retains all copyright and intellectual property in this data." Tell the user both
  statements exist before they build a redistribution product on it.
- **The "Solar home electricity data" dataset is gone.** The widely cited 300-home
  gross-metered 2010-2013 dataset 404s at its long-published URL. Do not link to it as
  if live.
- **Customer meter data is not obtainable programmatically.** A customer's own interval
  data requires a web form with NMI + account holder name + postcode verification (a
  signed consent form for third parties) and takes 10 business days for one NMI, up to
  20 for several. Australia's Consumer Data Right does not help here: it designates
  electricity **retailers** as data holders with AEMO as gateway, and Ausgrid is not in
  the CDR energy register. Route the user to their retailer for CDR data.
- **Do not build on `https://www.ausgrid.com.au/api/*`.** Those routes power the
  website's own outage map; they are undocumented, unversioned and carry no terms.
