# Home Climate Panel — Tado Log

A single, self-contained HTML dashboard for viewing temperature, humidity,
and heating history logged from a Tado X home. No backend, no build step —
just a static `index.html` served straight from GitHub Pages.

**[View the live dashboard →](../../)**

## What it does

The page automatically loads year-based CSV files (e.g. `2026_data.csv`)
sitting next to it in this repo, then renders:

- **Current readouts** — live temperature, humidity, and heating state,
  each with min / average / max for whatever time range is selected
- **A dual-axis line chart** — temperature and humidity plotted together,
  with heating periods shown as shaded bands underneath
- **A daily breakdown table** — min / avg / max for every day in range

Data is refreshed independently by a separate fetch script and GitHub
Actions workflow (see the private companion repo) roughly every 30
minutes; this repo just displays what's been synced.

## Features

- **Time range presets** — Day, Week, Month, Year, or a custom date range
- **Toggleable series** — click a readout tile (Temperature / Humidity /
  Heating) or a chart legend item to hide/show that line — handy for
  focusing on one metric at a time
- **Multi-room support** — a room selector appears automatically if the
  CSV contains more than one room
- **Automatic year rollover** — loads the current year's file, and the
  previous year's if needed to satisfy a selected date range
- **UK time handling** — all timestamps are bucketed and displayed using
  Europe/London civil time, so readings land on the correct day across
  the GMT/BST clock change regardless of the viewer's own device timezone
- **Manual CSV upload** — drop in your own CSV to explore other data
  without touching the repo
- **Mobile-friendly** — controls and layout adapt to small screens

## Data format

The dashboard expects one or more `YYYY_data.csv` files in the same
folder as `index.html`, with these columns:

| column           | description                                  |
|------------------|-----------------------------------------------|
| `date`           | calendar date (informational; London date is recalculated from `timestamp`) |
| `room`           | room name                                    |
| `timestamp`      | ISO 8601 UTC timestamp of the reading         |
| `temperature_c`  | temperature in °C                             |
| `humidity_pct`   | relative humidity in %                        |
| `heating_pct`    | heating power percentage (optional; omit or leave blank if not tracked) |

A new file is expected each calendar year (`2026_data.csv`,
`2027_data.csv`, and so on).

## Tech

Everything runs client-side in the browser:

- [Chart.js](https://www.chartjs.org/) for the chart, with a small custom
  plugin to draw the heating-shading bands
- [PapaParse](https://www.papaparse.com/) for CSV parsing
- Vanilla JS/HTML/CSS — no framework, no build step

## Local development

Just open `index.html` in a browser, or serve the folder with any static
file server, e.g.:

```bash
python3 -m http.server
```

Then visit `http://localhost:8000`.

## Related

- The private companion repo handles authenticating with Tado, polling
  the API on a schedule, and committing/mirroring the CSVs that this
  dashboard reads.
