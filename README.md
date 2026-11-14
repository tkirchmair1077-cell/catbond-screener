# Cat-bond screener

Public Phase 1 GitHub Pages site for [tkirchmair1077-cell/catbond-screener](https://github.com/tkirchmair1077-cell/catbond-screener), served at <https://tkirchmair1077-cell.github.io/catbond-screener/>.

The centrepiece is a rotatable globe (with a flat-map toggle) of recent earthquakes relevant to catastrophe-bond geographic screening. Circles are sized by magnitude and coloured by the USGS PAGER overall alert level. Hovering a circle shows the snapshot’s PAGER modelled economic-loss display text; clicking a circle or a table row opens magnitude, depth, place, time in UTC and Europe/Zurich, PAGER level, the USD figure, the USGS event link, the overlap status from the snapshot, any `overlapping_public_bonds` or `status_note` fields when present, and historical comparisons (PAGER published comps and USGS catalog analogs) with source links. Events with a published point estimate of at least USD 1 million also carry a short map label.

Translucent overlays are publicly sourced coverage regions from `data/zones.json`. Hovering or clicking a region opens a popup that lists the zone’s `note` verbatim and each entry in `properties.bonds`. For the Mexico earthquake zone the card still shows name, class, ISIN, issuer, insured, issue size, maturity, the trigger wording quoted and attributed to the World Bank press release, and http(s) links to the press release and the prospectus, labelled as publicly issued / publicly traded. For the Jamaica named-storm demo zone the card shows the public bond summary, the label “public summary, not verified against offering circular”, a demo/example flag, and the relevance state `REPORTED TRIGGER OUTCOME` with a link to the World Bank citation.

When `data/storms.json` is present, the map also draws a tropical-cyclone layer: official NHC cones and forecast tracks (advisories 11 and 19 for the Melissa demo), Google DeepMind Weather Lab ensemble member tracks as thin lines (one set per initialisation), and the NHC observed best track as a bold line. A layer toggle and legend are provided. Visible credits include: “Google DeepMind Weather Lab, CC BY 4.0, experimental research data, not official”; “Cone is not the wind, surge or rain footprint”; and an NHC public-domain credit. Open the demo at `?view=melissa`.

The USD figure uses the snapshot field `events[].pager_economic_loss_usd.display`. Wherever it appears, the page labels it “USGS PAGER modelled economic loss: all losses, not insured loss, not a bond trigger.” The 50% and 90% ranges are this desk’s own calculation (d). Missing values are shown as n/a. Insured-loss figures are not shown.

An “Assumptions and scale” section renders `data/assumptions.json`: the USGS PAGER Earthquake Impact Scale (quoted, with source), the PAGER economic-USD method and units (including the verified bin-to-USD conversion and the desk’s own (d) quantile method), the ALERT/WATCH magnitude bands marked as our own screening rule (d), every scenario figure tagged ASSUMPTION, and, when present, the tropical-cyclone demo methods and caveats (`tc_assumptions`). Scenario-module assumptions are not used in any Phase 1 event calculation; the economic-USD method is used for the public figures.

This page is a public research view. It is not investment advice. Economic loss, insured (industry) loss and bond principal impairment are kept separate. The page does not publish numeric principal-impairment estimates, deal terms, licensed industry-loss data, or insured-loss figures.

## Files to publish (repo root)

| Path | Role |
|------|------|
| `index.html` | The page (HTML, CSS, and vanilla JS). Uses d3 7.9.0 and topojson-client 3.1.0 from jsDelivr. |
| `data/events.json` | Public event snapshot written by Cat Bond Desk (`screen.py run` / `site`). Object with an `events` array. |
| `data/zones.json` | Public coverage regions as a GeoJSON FeatureCollection. Each feature must carry a public source URL. A feature may list publicly issued bonds in `properties.bonds`. |
| `data/meta.json` | `generated_at` (UTC and Europe/Zurich), window, thresholds, sources, version, counts, caveats. |
| `data/assumptions.json` | PAGER scale (verified from USGS), PAGER economic-USD method, screening bands (our rule, d), scenario assumptions, three-way loss separation, historical-comp rules, and tropical-cyclone demo methods (`tc_assumptions`). Version `phase1-0.2-tc`. |
| `data/storms.json` | Tropical-cyclone demo snapshot (schema `catbond-storms/1`). Official NHC cones, forecast tracks and best track; Google Weather Lab ensemble members; zone assessment; sources with retrieval time and licence. |
| `data/land-110m.json` | Natural Earth 110m land (via [world-atlas](https://github.com/topojson/world-atlas) 2.0.2), public-domain coastlines for the globe. |
| `README.md` | This file. |

The page fetches `data/*.json` with `cache: 'no-store'`. Do not invent events. Do not put licensed industry-loss indices, broker data, deal terms (attachment, exhaustion, franchise, hours clause, exact parametric boxes) or insured-loss estimates in these files.

## Who may write what

- Cat Bond Desk owns the snapshot files and writes them to `/workspace/catbond/site/`. For a publish, those files are copied into `data/` unchanged; they are not edited by hand.
- The page (`index.html`) does not recompute overlap status. It displays the `status` field as published (`no overlap`, `overlap, assessment pending`, or `threshold met`).
- `threshold met` is only used when a public basis is stated. Phase 1 has no such criterion.

## Snapshot in this edition

Earthquake events were copied unchanged from the Cat Bond Desk site files of 12 Nov 2026, 21:58 Europe/Zurich (CET). The tropical-cyclone demo files (`storms.json`, updated `zones.json`, `meta.json`, `assumptions.json`) were copied unchanged from the Cat Bond Desk site files of 14 Nov 2026, 08:47 Europe/Zurich (CET), data version `phase1-0.2-tc`:

- 26 events, M ≥ 5.8 (12 alert band M>6.0, 14 watch band 5.8≤M≤6.0)
- PAGER: 25 green, 1 orange (us6000u43g, M 7.0, 189 km ENE of Nagqu, China)
- `zones.json` has two features. The Mexico earthquake outline is unchanged: a simplified country outline (d) for display, not the contractual covered area, listing the publicly issued IBRD CAR Mexico 2024 earthquake bond, Class A (XS2799645515) and Class B (XS2799645606), with issuer, insured (Government of Mexico), issue size, 24 April 2028 maturity, trigger wording quoted from the World Bank press release of 17 April 2024, and links to that press release and the prospectus. The Jamaica named-storm outline is a coarse 10-vertex country outline (d) for a demo/example of the IBRD CAR Jamaica 2024 bond. Bond fields are a public summary, not verified against an offering circular. The zone carries relevance state `REPORTED TRIGGER OUTCOME` citing the World Bank press release of 7 November 2025. The outline is not contractual coverage; a track entering it is a watch condition, not a payout.
- `storms.json` holds the Hurricane Melissa (AL132025) demo: NHC 5-day cones and forecast tracks for advisories 11 and 19, the NHC ATCF best track, and two Google Weather Lab OPER ensemble initialisations (2025-10-24 00 UTC and 2025-10-26 00 UTC, 50 members each, thinned to 120 h). Official NHC products are public domain. Weather Lab tracks are experimental research data, not official, CC BY 4.0 Historical Data.
- Every earthquake event status in this snapshot is `no overlap`. No current event carries `overlapping_public_bonds` or `status_note`. Those fields are rendered in the event tooltip and detail panel when present.
- Each event carries `pager_economic_loss_usd` (display text, point estimate, bins, and p50/p90 ranges) and `historical_comps`: PAGER historical comparison events (as published, with a source URL) and USGS catalog analogs (M≥6.0 within 250 km since 1900; up to the five largest plus a count). The catalog radius, floor, start year and “five largest” cap are our own choice (d), not a USGS rule.
- A note on the page, not in `events.json`, records that [us6000u18k](https://earthquake.usgs.gov/earthquakes/eventpage/us6000u18k) (M 7.7, 10 km WSW of Pitaloza Arriba, Panama, 9 Oct 2026 17:56 UTC) sits 3 days 22 hours before the window start and is not plotted.

## Schema (events)

`events.json` is an object `{ "events": [ ... ] }`, not a GeoJSON FeatureCollection. Each object in `events[]` has: `id`, `usgs_event_page`, `time_utc`, `time_europe_zurich`, `magnitude`, `mag_type`, `depth_km`, `latitude`, `longitude`, `place`, `updated_utc`, `updated_europe_zurich`, `review_status`, `band` (`alert` or `watch`), `pager_alert_level` (`green` / `yellow` / `orange` / `red` / `null`) with `pager_alert_level_reason` when null, `shakemap_url`, `status`, `historical_comps`, and `pager_economic_loss_usd`. When an epicentre overlaps a public zone that lists publicly issued bonds, the record may also carry `overlapping_public_bonds` (display names) and `status_note`. This snapshot has neither field on any event.

`zones.json` is a GeoJSON FeatureCollection. Each feature’s `properties` may include `name`, `note`, a public `source_url`, and a `bonds` array of publicly issued bonds. Mexico-style bond objects carry name, class, ISIN, issuer, insured, issue size, scheduled maturity, trigger wording, and press-release and prospectus URLs. Jamaica-style demo objects carry a public-summary `label`, a `demo` flag, and World Bank-cited fields (issuer, sponsor, peril, trigger type, size, issue date, status); they are not verified against an offering circular. A tropical-cyclone zone may also carry `tc_relevance` (`state`, `reason`, `source_url`). The outline geometry is for display only.

`storms.json` is an object `{ "schema": "catbond-storms/1", "storms": [ ... ], "sources": { ... }, "notes": [ ... ] }`. Each storm may include `official.advisories[]` (cone polygon, forecast track points), `observed.points` (best track), `google_ensemble.initialisations[]` (members with `coords` as lon/lat arrays), and `zone_assessment`. Each entry in `sources` carries a retrieval timestamp and a licence field.

`pager_economic_loss_usd` carries `display`, `point_estimate_usd`, `bins`, `p50_range_usd`, `p90_range_usd`, and related method fields. The page shows `display` and does not recompute the figure. `p50_range_usd` and `p90_range_usd` are the desk’s own calculation (d).

`historical_comps.pager_historical` lists comparison events as published in the USGS losspager product, with `source_url`. `historical_comps.catalog_analogs` lists up to five catalog events (`listed`) plus `count` and `query_url`.

PAGER colour is the USGS overall alert (the higher of the fatality and economic alerts) as published on the event. It is not an insured-loss figure and not a contract result.

## Local preview

Serve the folder over HTTP (fetch will not work from `file://`):

```bash
python3 -m http.server 8766 --directory /workspace/catbond/pages
```

Then open <http://127.0.0.1:8766/>. Append `?view=melissa` to centre the tropical-cyclone demo and open the Jamaica zone popup.

## Sources

- USGS Earthquake Hazards Program / ComCat, FDSN event API: <https://earthquake.usgs.gov/fdsnws/event/1/>
- USGS PAGER: <https://earthquake.usgs.gov/data/pager/>
- PAGER Scientific Background (Earthquake Impact Scale): <https://earthquake.usgs.gov/data/pager/background.php>
- Land: Natural Earth 110m, packaged by world-atlas 2.0.2
- NOAA National Hurricane Center GIS forecast archive and ATCF best-track archive (U.S. Government work, public domain)
- Google DeepMind Weather Lab / WeatherNext Cyclones ensemble tracks, CC BY 4.0 (Historical Data); experimental research data, not official
- World Bank press release, 7 November 2025, on the Hurricane Melissa trigger outcome for the Jamaica 2024 cat bond
- Jamaica and Mexico country outlines: public GeoJSON (johan/world.geo.json), simplified for display

Magnitudes, locations, depths, PAGER levels and PAGER loss estimates can be revised by USGS. Each `meta.json` is a snapshot at `generated_at_utc`. The tropical-cyclone demo also records `updated_at_utc`.
