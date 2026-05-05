---
name: Static Map Generation Pattern
description: Quick visual maps with markers using staticmap + PIL overlay. Used for trip planning, location research, any "mark this on a map" task.
type: reference
related: [reference_ffmpeg.md, project_rasclaw_telegram.md]
---

Build annotated maps from Python without any paid API (Google Maps, Mapbox).

## Stack
- `staticmap` (pip install) -- pulls OSM tiles, renders to PIL Image
- PIL/Pillow -- add title bar, legend, numbered markers, sight lines on top
- OpenStreetMap tiles (free, must set User-Agent header)

## Pattern
```python
from staticmap import StaticMap, CircleMarker
from PIL import Image, ImageDraw, ImageFont
import math

m = StaticMap(1280, 960, url_template='https://a.tile.openstreetmap.org/{z}/{x}/{y}.png',
              headers={'User-Agent': 'YourApp/1.0'})

# Add invisible markers just to set extents
for lat, lon in points:
    m.add_marker(CircleMarker((lon, lat), '#00000000', 1))

image = m.render(zoom=16).convert("RGBA")
```

Then project lat/lon to pixel using Web Mercator (center = midpoint of extents) and draw custom markers/labels with PIL.

Full working example lives in `DuberyMNL/.tmp/pyro_map.png` generation code (session 98 -- Pyro Musical viewing spots).

## Why not folium / Google Maps
- Folium = interactive HTML, can't easily send as image to Telegram
- Google Maps Static API = needs API key + billing
- staticmap + OSM = free, pure Python, outputs PNG directly

## Zoom reference
- 14 = district level
- 16 = neighborhood (MOA area fits perfectly)
- 18 = single building

## Tip
staticmap centers on the midpoint of all added markers. If you want a specific center, add invisible anchor markers at the corners you want framed.
