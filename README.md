# Earthquake cat-bond screener

Public Phase 1 GitHub Pages site for [tkirchmair1077-cell/catbond-screener](https://github.com/tkirchmair1077-cell/catbond-screener), served at <https://tkirchmair1077-cell.github.io/catbond-screener/>.

The centrepiece is a rotatable globe (with a flat-map toggle) of recent earthquakes relevant to catastrophe-bond geographic screening. Circles are sized by magnitude and coloured by the USGS PAGER overall alert level. Clicking a circle or a table row opens magnitude, depth, place, time in UTC and Europe/Zurich, PAGER level, the USGS event link, and the overlap status from the snapshot.

This page is a public research view. It is not investment advice. Economic loss, insured (industry) loss and bond principal impairment are kept separate. The page does not publish numeric principal-impairment estimates, deal terms, or licensed industry-loss data.

## Files to publish (repo root)

| Path | Role |
|------|------|
| `index.html` | The page (HTML, CSS, and vanilla JS). Uses d3 7.9.0 and topojson-client 3.1.0 from jsDelivr. |
| `data/events.json` | Public event snapshot written by Cat Bond Desk (`screen.py run` / `site`). |
| `data/zones.json` | Public coverage regions as a GeoJSON FeatureCollection. Empty until a region is verified from a primary public source. |
| `data/meta.json` | `generated_at` (UTC and Europe/Zurich), window, thresholds, sources, version, counts, caveats. |
| `data/land-110m.json` | Natural Earth 110m land (via [world-atlas](https://github.com/topojson/world-atlas) 2.0.2), public-domain coastlines for the globe. |
| `README.md` | This file. |

The page fetches `data/*.json` with `cache: 'no-store'`. Do not invent events. Do not put licensed industry-loss indices, broker data, deal terms (attachment, exhaustion, franchise, hours clause, exact parametric boxes) or loss estimates in these files.

## Who may write what

- Cat Bond Desk owns the three snapshot files and writes them to `/workspace/catbond/site/`. For a publish, those files are copied into `data/`; they are not edited by hand.
- The page (`index.html`) does not recompute overlap status. It displays the `status` field as published (`no overlap`, `overlap, assessment pending`, or `threshold met`).
- `threshold met` is only used when a public basis is stated. Phase 1 has no such criterion.

## Snapshot in this edition

Copied from the Cat Bond Desk one-shot 30-day run of 12 Nov 2026, 17:25 Europe/Zurich (CET):

- 26 events, M ≥ 5.8 (12 alert band M>6.0, 14 watch band 5.8≤M≤6.0)
- PAGER: 25 green, 1 orange (us6000u43g, M 7.0, 189 km ENE of Nagqu, China)
- Every event status is `no overlap`, because `zones.json` is an empty FeatureCollection — no publicly disclosed coverage region was verified from a primary public source. The map legend says so plainly: “No verified coverage zones yet.”
- A note on the page, not in `events.json`, records that [us6000u18k](https://earthquake.usgs.gov/earthquakes/eventpage/us6000u18k) (M 7.7, 10 km WSW of Pitaloza Arriba, Panama, 9 Oct 2026 17:56 UTC) sits 3 days 22 hours before the window start and is not plotted.

## Schema (events)

Each object in `events[]` has: `id`, `usgs_event_page`, `time_utc`, `time_europe_zurich`, `magnitude`, `mag_type`, `depth_km`, `latitude`, `longitude`, `place`, `updated_utc`, `updated_europe_zurich`, `review_status`, `band` (`alert` or `watch`), `pager_alert_level` (`green` / `yellow` / `orange` / `red` / `null`) with `pager_alert_level_reason` when null, `shakemap_url`, and `status`.

PAGER colour is the USGS overall alert (the higher of the fatality and economic alerts) as published on the event. It is not an insured-loss figure and not a contract result.

## Local preview

Serve the folder over HTTP (fetch will not work from `file://`):

```bash
python3 -m http.server 8766 --directory /workspace/catbond/pages
```

Then open <http://127.0.0.1:8766/>.

## Sources

- USGS Earthquake Hazards Program / ComCat, FDSN event API: <https://earthquake.usgs.gov/fdsnws/event/1/>
- USGS PAGER: <https://earthquake.usgs.gov/data/pager/>
- Land: Natural Earth 110m, packaged by world-atlas 2.0.2

Magnitudes, locations, depths and PAGER levels can be revised by USGS. Each `meta.json` is a snapshot at `generated_at_utc`.
