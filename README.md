## About The Project

This repository contains the source code for the **Interactive Map & Providential Timeline of Sun Myung Moon and Hak Ja Han Moon**, developed as a core visual component of the [True Parents Legacy Archive](https://tplegacy.net).

By leveraging modern web mapping technologies and structured chronological data, this project translates decades of complex global mission history into an intuitive, interactive spatial-temporal experience. It serves as an open-access tool for independent researchers, theologians, and historians to trace the geographical expansion and pivotal turning points of the Unification providence.

### Key Features
* **Interactive Geo-Tracking:** Visualizes historical travels, speech tours, and the institutional founding locations across the globe.
* **Providential Chronology:** A comprehensive, searchable timeline mapped across the distinct eras of Unification history (Old, New, and Completed Testament dispensations).
* **Source-Linked Data:** Every milestone and map node is fully cited and cross-referenced with the official speech corpus (*Malsseum*) and canonical historical texts.


demo
https://tplegacy.net/timeline/



# True Parents — historical event locations

> Open dataset of geolocated events from the lives and ministry of **Sun Myung Moon** (1920–2012) and **Hak Ja Han Moon** (1943–present), founders of the Unification Movement.

[![Data license: CC BY-SA 4.0](https://img.shields.io/badge/data-CC%20BY--SA%204.0-blue.svg)](https://creativecommons.org/licenses/by-sa/4.0/)
[![Code license: MIT](https://img.shields.io/badge/code-MIT-green.svg)](#license)

**4,936 located events across 279 sites worldwide, 1920–2025.**

🌐 **Live demo:** [tplegacy.github.io/tp-event-map](https://tplegacy.github.io/tp-event-map/) *(replace with your actual GitHub Pages URL)*
📚 **Source archive:** [tplegacy.net/timeline](https://tplegacy.net/timeline/)

---

## What's in this repository

| File | What it is | Use it for |
|---|---|---|
| **`tp-locations.json`** | The dataset — 279 locations with lat/lng, event counts per decade, and counts per event type. | Building your own visualisations, statistical analysis, citing in research, contributing corrections. |
| **`index.html`** | A self-contained reference visualisation using [Leaflet](https://leafletjs.com/). Open it in a browser or host it on GitHub Pages. | Embedding the live map, learning how to consume the JSON, forking for your own design. |

That's it — two files.

---

## Quick start

### View the map

```bash
git clone https://github.com/YOUR_USER/tp-event-map.git
cd tp-event-map
python3 -m http.server 8000        # or: npx serve
```

Then open <http://localhost:8000/>. (A local server is needed because the HTML uses `fetch()` for the JSON — opening `index.html` directly via `file://` will fail with a CORS error.)

### Host on GitHub Pages

1. Push both files to a public repo.
2. **Settings → Pages → Source:** *Deploy from a branch* → `main` / `/ (root)`.
3. Your map is live at `https://YOUR_USER.github.io/REPO_NAME/`.

### Use the data in your own project

```js
const dataset = await fetch('https://YOUR_USER.github.io/REPO_NAME/tp-locations.json')
  .then(r => r.json());

console.log(dataset.metadata.totalEvents);     // 4936
console.log(dataset.locations[0].name);        // "Hannam-dong, Seoul"
console.log(dataset.locations[0].by_decade);   // { "1980": 31, "1990": 54, "2000": 467 }
```

```python
import requests
ds = requests.get('https://YOUR_USER.github.io/REPO_NAME/tp-locations.json').json()
print(len(ds['locations']), 'sites')
```

---

## Data schema

The JSON file is a single object with five top-level keys: `metadata`, `schema`, `decades`, `eventTypes`, `locations`.

### `locations[]`

```json
{
  "name": "Hannam-dong, Seoul",
  "lat": 37.5326,
  "lng": 127.0086,
  "total": 552,
  "by_decade": { "1980": 31, "1990": 54, "2000": 467 },
  "by_type":   { "rally": 43, "tour": 1, "assembly": 1,
                 "conference": 2, "proclamations": 14,
                 "blessing": 1, "festival": 1 }
}
```

| Field | Type | Notes |
|---|---|---|
| `name` | string | Display name — city, region, or named location |
| `lat`, `lng` | number | Decimal degrees, WGS84 |
| `total` | integer | Total events at this site across all decades and types |
| `by_decade` | object | `"1970"` → count. Decades that don't appear had no events. |
| `by_type` | object | `key` → count. Keys match `eventTypes[].key`. |

### `eventTypes[]`

`rally` · `proclamations` · `festival` · `assembly` · `conference` · `blessing` · `tour`

- **`proclamations`** merges entries marked *proclamation* and *declaration* in source data.
- `by_type` counts may sum to less than `total` when some events lack a typed category.

### Notes on data quality

- Entries named **`<Country> (general)`** (e.g. *"Korea (general)"*, *"USA (general)"*, *"Japan (general)"*) aggregate events whose location was recorded only at country level in the source archive.
- Coordinates for country-level entries are approximate (country centroid).
- Some historical North Korean locations use modern coordinates of place names that may have changed.

---

## Contributing

Corrections, additions, and translations are welcome.

### To correct a location

Open an issue or pull request against `tp-locations.json` with:
- The location name as it appears in the file
- The proposed change (coordinates, name, event count)
- A source citation (page in *World Scripture and the Teachings of Sun Myung Moon*, sermon date, news archive, etc.)

### To add a new site or event

Same — open an issue with the proposed entry and at least one source citation. We try to keep `total` as the ground truth and `by_decade` / `by_type` as breakdowns; both must be updated consistently.

---

## License

- **Data (`tp-locations.json`):** [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/). You may share and adapt the data for any purpose, including commercially, provided you give appropriate credit and distribute your contributions under the same license.
- **Code (`index.html`):** MIT.

### Attribution

If you use this dataset, please cite:

> *True Parents Legacy archive (2026). Sun Myung Moon & Hak Ja Han Moon historical event locations, v1.0.0. https://tplegacy.net/timeline/. CC BY-SA 4.0.*

---

## About the source

This data is compiled by the [True Parents Legacy](https://tplegacy.net/) project — an independent digital archive preserving the teachings, sermons, and historical record of the founders of the Unification Movement. The archive is **not affiliated with FFWPU or HSA-UWC**; content is presented for scholarship and historical research under fair use.

For source citations, primary documents, and sermon transcripts behind each event, see the [complete timeline](https://tplegacy.net/timeline/).

