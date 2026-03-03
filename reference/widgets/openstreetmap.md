# OpenStreetMap Widget

An embedded OpenStreetMap widget with customizable coordinates, zoom, and map style. No API key required.

## Widget Type

```
openstreetmap
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"openstreetmap"` |
| `uuid` | string | Unique widget identifier |
| `config` | object | Widget configuration |
| `config.lat` | number | Latitude coordinate (e.g., `52.2297`) |
| `config.lng` | number | Longitude coordinate (e.g., `21.0122`) |
| `config.zoom` | number | Zoom level (1-18, default: 14) |
| `config.tile_style` | string | Map tile style: `"standard"`, `"light"`, `"dark"` |
| `config.show_marker` | boolean | Show pin marker at coordinates |
| `config.scroll_wheel_zoom` | boolean | Enable zoom via scroll wheel |
| `config.zoom_control` | boolean | Show zoom +/- buttons |
| `config.draggable` | boolean | Allow map panning by dragging |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "openstreetmap",
  "uuid": "osm-123",
  "config": {
    "lat": 52.2297,
    "lng": 21.0122,
    "zoom": 14,
    "tile_style": "standard",
    "show_marker": true,
    "scroll_wheel_zoom": false,
    "zoom_control": true,
    "draggable": true
  },
  "settings": {
    "minHeight": 400,
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Tile Styles

| Value | Description |
|-------|-------------|
| `standard` | Default OpenStreetMap tiles |
| `light` | Light/minimal map style |
| `dark` | Dark mode map style |

## Zoom Levels

| Range | Description |
|-------|-------------|
| 1-5 | World / Continent view |
| 6-10 | Country / Region view |
| 11-14 | City view |
| 15-17 | Street view |
| 18 | Building view |

## Usage Example

```javascript
function renderOpenStreetMap(widget) {
  const {
    lat, lng, zoom,
    tile_style,
    show_marker,
    scroll_wheel_zoom,
    zoom_control,
    draggable
  } = widget.config;

  const height = widget.settings?.minHeight || 400;

  const tileUrls = {
    standard: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
    light: 'https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}{r}.png',
    dark: 'https://{s}.basemaps.cartocdn.com/dark_all/{z}/{x}/{y}{r}.png'
  };

  const mapId = `map-${widget.uuid}`;

  return `
    <div id="${mapId}" style="width: 100%; height: ${height}px;"></div>
    <script>
      (function() {
        const map = L.map('${mapId}', {
          center: [${lat || 52.2297}, ${lng || 21.0122}],
          zoom: ${zoom || 14},
          scrollWheelZoom: ${!!scroll_wheel_zoom},
          zoomControl: ${zoom_control !== false},
          dragging: ${draggable !== false}
        });

        L.tileLayer('${tileUrls[tile_style] || tileUrls.standard}', {
          attribution: '&copy; OpenStreetMap contributors'
        }).addTo(map);

        ${show_marker ? `L.marker([${lat}, ${lng}]).addTo(map);` : ''}
      })();
    </script>
  `;
}
```

> **Note**: This widget requires [Leaflet.js](https://leafletjs.com/) library to be loaded on the page. Include `leaflet.css` and `leaflet.js` in your HTML head.
