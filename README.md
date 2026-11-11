# GPX Viewer

A single-page web app for viewing GPX files on a map. This is handy if you want to quickly review waypoints you took on your GPS after a field day and need to copy them. Load one or more GPX files and see their
tracks, routes and waypoints on a choice of basemaps, with distance, duration, elevation stats and
coordinates in both lat/long and UTM.

**Live site:** https://mclairewrazej-goatgirl.github.io/GPX-Viewer/

## How to use it

1. **Load files.** Click **Choose GPX files**, or drag one or more `.gpx` files onto the page.
   You can load several at once; each gets its own colour. Want something to try first? Download
   [`samples/example.gpx`](samples/example.gpx) (a made-up hike near Banff, AB) and load it.
2. **Sort.** Use the **Sort** menu in the sidebar to order files by date (oldest or newest first),
   by name, or in the order you added them. A file's date is the earliest timestamp in it, or the
   `<metadata><time>` if its points have no times. Files with no date are listed last.
3. **Manage files.** For each file you can zoom to it (⌖), show/hide it (👁), remove it (✕), or
   click the coloured dot to change its colour. **Zoom to all** and **Clear all** are at the bottom.
4. **Switch layers.** Use the layer switcher (stacked squares icon, top right of the map) to pick a
   basemap and to turn the **Place labels** overlay on or off (handy over satellite imagery).
5. **Get coordinates.**
   - Move the mouse over the map: the box in the bottom-right shows the cursor's position.
   - Click anywhere on the map for a popup with that point's coordinates.
   - Click a track or route for its stats plus the time, elevation and coordinates of the nearest
     recorded point. Click a start/end dot or a waypoint for its details.
   - **Copy a value:** in any popup, click a number to copy just that number: the latitude, the
     longitude, the easting or the northing. You get digits only, without the zone or the E/N
     letters. For example, `11U 600165E 5670640N` copies as `600165` (easting) or `5670640` (northing).

Tracks are drawn as solid lines and routes as dashed lines. Start points are filled dots and end
points are hollow dots.

## Privacy

Your GPX files never leave your browser. They are read and parsed entirely on your device with
JavaScript; nothing is uploaded to any server. (The page does download map tiles from the basemap
providers below, as any web map does, which tells them which area of the map you're viewing.)

## Coordinates

Every point is shown two ways. Here is the "Trailhead" waypoint from the sample file:

```
Lat/Long  51.178400, -115.567000
UTM       11U 600165E 5670640N
```

**Lat/Long** (latitude and longitude, in degrees)

| Part | Meaning |
| --- | --- |
| `51.178400` | Latitude: degrees north of the equator (a minus sign would mean south). |
| `-115.567000` | Longitude: degrees east of Greenwich. The minus sign means **west**, which is normal for anywhere in Canada or the US. |

**UTM** (a grid measured in metres)

| Part | Meaning |
| --- | --- |
| `11` | The **zone**. UTM splits the world into 60 strips running north–south, each 6° of longitude wide. Banff is in zone 11. |
| `U` | The **latitude band**, a row letter. It is *not* "north" or "south". Letters N to X are all in the northern hemisphere, and C to M are in the southern. |
| `600165E` | The **easting**: metres east, measured within the zone. |
| `5670640N` | The **northing**: metres north of the equator. |

**If a GPS or app asks for "zone and hemisphere"**, enter zone **11** and hemisphere **N** (north).
Don't enter the band letter there: band "U" is not a hemisphere, and in this format "11N" means
"zone 11, north", not "band N".

**Copying:** clicking the easting or northing copies only the digits (`600165` or `5670640`),
without the zone or the E/N letters.

**Good to know**

- The coordinates use **WGS84**, the same system as GPS. NAD83 (used by most current Canadian and
  US maps) matches WGS84 to within a metre or two. Older paper maps marked **NAD27** can be off by
  about 200 m.
- UTM doesn't cover the polar regions (beyond 84° N or 80° S), so "n/a (polar)" is shown there.

## Basemap sources and attribution

| Layer | Source | Attribution |
| --- | --- | --- |
| OpenStreetMap (CARTO) | CARTO Voyager tiles | © [OpenStreetMap](https://www.openstreetmap.org/copyright) contributors © [CARTO](https://carto.com/attributions) |
| OpenStreetMap (standard) | openstreetmap.org tiles | © [OpenStreetMap](https://www.openstreetmap.org/copyright) contributors |
| OpenTopoMap | opentopomap.org | Map data © OpenStreetMap contributors, SRTM; style © [OpenTopoMap](https://opentopomap.org) (CC-BY-SA) |
| Satellite (Esri) | Esri World Imagery | Imagery © Esri, Maxar, Earthstar Geographics |
| Satellite (Sentinel-2, 10 m) | EOX Sentinel-2 cloudless 2020 | [Sentinel-2 cloudless](https://s2maps.eu) by EOX IT Services GmbH (contains modified Copernicus Sentinel data 2020) |
| Place labels (overlay) | Esri World Boundaries and Places | Labels © Esri |

The map itself uses [Leaflet](https://leafletjs.com/) 1.9.4, loaded from cdnjs.

When the page is opened from the website it starts on OpenStreetMap (standard); when `index.html`
is opened as a local file it starts on OpenStreetMap (CARTO), because OSM's own tile servers block
pages opened from `file://`.

## Updating the site

The whole app is the single file `index.html`. To change it:

1. Edit `index.html` (open it directly in your browser to test locally).
2. Commit and push:

   ```bash
   git add index.html
   git commit -m "Describe your change"
   git push
   ```

GitHub Pages redeploys automatically from the `main` branch; the live site updates within a minute
or two. The empty `.nojekyll` file tells GitHub Pages to serve the files as-is, so leave it in place.
