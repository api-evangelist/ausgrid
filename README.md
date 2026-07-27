# Ausgrid (ausgrid)

Ausgrid is the largest electricity distribution network service provider on Australia's east coast, operating the poles, wires, substations and underground cables that deliver power to more than 1.8 million customers across Sydney, the Central Coast and the Hunter Valley in New South Wales. It sits in the regulated middle of the value chain — between the National Electricity Market and the retailers who bill the customer — and it earns a regulated revenue rather than selling energy. Its API posture is honestly split and worth stating plainly. On the open side, Ausgrid publishes genuinely open network data: twenty years (2005–2025) of 15-minute interval demand readings for more than 180 zone substations as freely downloadable zipped CSV, plus average electricity use and past outage datasets catalogued on the NSW Government CKAN portal under CC-BY, none of which require a login, key or agreement. On the consumer side it is closed: a customer's own interval meter data is obtained through a web form that verifies NMI, account name and postcode and is answered in 10 to 20 business days, with a signed consent form required for any third party — there is no consumer API. Australia's Consumer Data Right was extended to energy and is live, but the designation lands on electricity retailers as primary data holders with AEMO as the gateway; Ausgrid is a distributor and does not appear among the 84 energy brands in the public CDR Register, so the mandate proven in banking routes around the business that physically holds the meter. Ausgrid publishes no developer portal, no OpenAPI, and no documented API of any kind; the only machine-readable surfaces found are the undocumented internal JSON routes behind its own outage map.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/ausgrid/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/ausgrid/refs/heads/main/apis.yml)

## Tags

- Energy
- Australia
- Utilities
- Electricity
- Grid
- Distribution Network
- Open Data
- Smart Metering
- Consumer Data Right
- Solar
- DER
- Outages

## Timestamps

- **Created:** 2026-07-27
- **Modified:** 2026-07-27

## APIs

No documented public API. Ausgrid publishes no developer portal, no OpenAPI or other machine-readable contract, and no API reference. See [`review.yml`](review.yml) for every URL probed and the HTTP status returned.

Ausgrid-owned network planning and unallocated hosting capacity data *is* queryable — as anonymous ArcGIS REST FeatureServer layers on the **NSW Government** portal (`portal.data.nsw.gov.au`), not on an Ausgrid host. Every feature carries `owner="Ausgrid"`, but the endpoint belongs to NSW Government, so it is catalogued here as Data and Reference, never as an Ausgrid API. Service and layer metadata is harvested verbatim into [`arcgis/`](arcgis/) and live responses into [`examples/`](examples/).

## Artifacts

| Directory | What is in it |
| --- | --- |
| [`arcgis/`](arcgis/) | Verbatim ArcGIS FeatureServer and layer metadata for `Ausgrid_DTAPR_2023` (8,696 feeder/line features) and `Ausgrid_UHC_Data` (2,160 primary + 2,120 secondary hosting-capacity records) |
| [`examples/`](examples/) | Verbatim query responses, the ArcGIS error envelope, and one response from Ausgrid's own undocumented outage-map route |
| [`well-known/`](well-known/) | Every `/.well-known/` probe on all four Ausgrid hosts — all miss; documents the idoportal redirect-to-error-page false positive |
| [`security/`](security/) | Domain security probe (TLS/HSTS/CAA/SPF/DMARC) and the vulnerability disclosure program |
| [`authentication/`](authentication/) | No credential exists anywhere; consumer meter data is gated by identity verification, not auth |
| [`conventions/`](conventions/) | ArcGIS query, paging, format and error conventions; Ausgrid bulk-file conventions |
| [`errors/`](errors/) | Observed error envelopes (ArcGIS returns HTTP 200 with an error body) |
| [`lifecycle/`](lifecycle/) | Annual NER 5.13A publication cadence; no deprecation policy — the site restructure broke the catalogued links |
| [`conformance/`](conformance/) | What standards do and do not apply, with the evidence for each |
| [`data-model/`](data-model/) | Field-level model derived from the harvested layer schemas |
| [`vocabulary/`](vocabulary/) | NMI, DNSP, zone substation, DTAPR, UHC, N-1, CDR, MSATS and friends |
| [`packages/`](packages/) | No first-party SDK, no GitHub org; community tooling listed |
| [`skills/`](skills/) | Two agent skills: hosting-capacity lookup, and getting the interval demand data |
| [`llms/`](llms/) | Generated `llms.txt` (Ausgrid publishes none) |

## Mandate Posture

| Field | Value |
| --- | --- |
| Mandate regime | `other` — National Electricity Rules 5.13A annual planning data publication |
| Mandate status | `live-implemented` — the mandated interval data files exist and download today |
| Consumer Data Right (energy) | Live in Australia, **does not designate distributors**; Ausgrid is absent from the 84 energy brands in the public CDR Register |
| Data standard | no standard reference found (consumer meter data supplied per AEMO meter data provision procedures, CSV) |
| Consumer data API | No — web form, identity check, 10–20 business days |
| Market/network data open | Yes — 2005–2025 zone substation interval demand, free and anonymous |
| Access gate | `self-serve` for open data; identity-verified form for consumer data |
| Auth model | None on anything published; no API key, OAuth2, OIDC or mTLS |

## Properties

- [Website](https://www.ausgrid.com.au/)
- [About](https://www.ausgrid.com.au/about-us)
- [Blog](https://www.ausgrid.com.au/about-us/newsroom)
- [Privacy](https://www.ausgrid.com.au/Ausgrid-Privacy-Policy)
- [Vulnerability Disclosure](https://www.ausgrid.com.au/outages-and-issues/customer-support/ausgrid-vulnerability-disclosure-program)
- [LinkedIn](https://www.linkedin.com/company/ausgrid/)
- [Data — Research data sets](https://www.ausgrid.com.au/about-us/about-ausgrid/research-data-sets)
- [Data — Distribution zone substation data](https://www.ausgrid.com.au/about-us/about-ausgrid/research-data-sets/distribution-zone-substation-data)
- [Data — Average electricity use](https://www.ausgrid.com.au/about-us/about-ausgrid/research-data-sets/average-electricity-use)
- [Data — Electricity research](https://www.ausgrid.com.au/about-us/about-ausgrid/research-data-sets/electricity-research)
- [Data — Ausgrid on data.nsw.gov.au](https://data.nsw.gov.au/data/organization/ausgrid)
- [Documentation — Access your meter data](https://www.ausgrid.com.au/your-energy-use/your-meter-and-supply/access-your-meter-data)
- [Portal — Ausgrid Services sign in](https://services.ausgrid.com.au/SignIn)
- [Portal — IDO connection enquiries](https://idoportal.ausgrid.com.au/)
- [Outages](https://www.ausgrid.com.au/outages)

## Maintainers

- Kin Lane — kin@apievangelist.com
