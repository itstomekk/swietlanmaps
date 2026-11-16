# Development

The site is one static page, `docs/index.html`, plus overlay images in `docs/img/` (800 px WebP). It is served by GitHub Pages from `main` /docs. There is no build step in this repo and no runtime dependency.

## Data in the page

`index.html` embeds one JSON object (`const DATA = ...`):

| Key | Content |
|---|---|
| `powiaty` | 380 counties: `id`, `name`, `woj` (voivodeship), `d` (SVG path) |
| `woj` | 16 voivodeship outlines |
| `maps` | one entry per map: `word`, `meaning`, `kind`, `survey`, `source` (link to the original post), `img`, `concept`, `L`, `S` |
| `concepts` | groups of maps that share one meaning |

- `L` is a 380-character string, one digit per county in `powiaty` order: 0 none, 1 rare, 2 frequent, 3 common.
- `S` is 3 digits per county (0-9): the share of the county's area drawn as rare, frequent and common on the source graphic.
- Geometry is drawn in the source graphics' pixel space (2048 x 2048), so the original image placed at (0, 0, 2048, 2048) lines up with the counties.

## How the levels are made

The pipeline lives outside this repo. In short:

1. County borders are projected to EPSG:2180 and fitted to the Poland outline on the graphics (IoU 0.99).
2. Each pixel inside Poland is classed as red (common), orange (frequent), hatched (rare) or none. Borders are skipped.
3. Each county gets the highest level that covers enough of its area.

## Views

- `#m:<id>`: one word. `#c:<meaning>`: words that share a meaning. `#index`: regionality index.
- Arrow keys move through the list. Escape clears the selected county.
