# OpenStreetMap Widget

An embedded OpenStreetMap widget with customizable coordinates, zoom, map style, and GeoJSON overlay support. No API key required.

## Widget Type

```
openstreetmap
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"openstreetmap"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget data |
| `widget.lat` | number | Latitude coordinate (e.g., `52.2297`) |
| `widget.lng` | number | Longitude coordinate (e.g., `21.0122`) |
| `widget.zoom` | number | Zoom level (1-18, default: 14) |
| `widget.tile_style` | string | Map tile style: `"standard"`, `"light"`, `"dark"` |
| `widget.show_marker` | boolean | Show pin marker at coordinates |
| `widget.scroll_wheel_zoom` | boolean | Enable zoom via scroll wheel |
| `widget.zoom_control` | boolean | Show zoom +/- buttons |
| `widget.draggable` | boolean | Allow map panning by dragging |
| `widget.geojson_source` | string | GeoJSON data source: `"static"` (file URL) or `"dynamic"` (collection field) |
| `widget.geojson_file` | string | URL to a `.geojson` file (when `geojson_source` is `"static"`) |
| `widget.collection_code` | string | Collection code for dynamic GeoJSON source |
| `widget.entry_source` | string | How to resolve the entry: `"static"` (by ID) or `"url"` (from URL segment) |
| `widget.entry_id` | string | Entry ID (when `entry_source` is `"static"`) |
| `widget.entry_url_segment` | number | URL segment index for entry resolution (when `entry_source` is `"url"`) |
| `widget.geojson_field_code` | string | Field code containing GeoJSON data in the collection entry |
| `widget.geojson_fill_color` | string | Fill color for GeoJSON polygons (e.g., `"#3388ff"`) |
| `widget.geojson_stroke_color` | string | Stroke/border color for GeoJSON features (e.g., `"#3388ff"`) |
| `widget.geojson_fill_opacity` | string | Fill opacity for GeoJSON polygons (e.g., `"0.2"`) |
| `widget.collection_layers_enabled` | boolean | Enable loading GeoJSON from all entries in a collection |
| `widget.collection_layers_collection` | string | Collection code to load GeoJSON entries from |
| `widget.collection_layers_field` | string | Field code containing GeoJSON data in each collection entry |
| `widget.collection_layers_limit` | number | Maximum number of entries to load for collection layers (default: 50) |
| `widget.collection_layers_filter_field` | string | Field code to filter collection layer entries by |
| `widget.collection_layers_filter_value` | string | Value to match for filtering |
| `widget.collection_layers_filter_value_source` | string | Filter value source: `"static"` or `"url"` (default: `"static"`) |
| `widget.collection_layers_filter_url_segment` | number | URL path segment number to extract filter value from (default: `1`) |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "openstreetmap",
  "uuid": "osm-123",
  "widget": {
    "lat": 52.2297,
    "lng": 21.0122,
    "zoom": 14,
    "tile_style": "standard",
    "show_marker": true,
    "scroll_wheel_zoom": false,
    "zoom_control": true,
    "draggable": true,
    "geojson_source": "static",
    "geojson_file": "",
    "collection_code": "",
    "entry_source": "static",
    "entry_id": "",
    "entry_url_segment": 1,
    "geojson_field_code": "",
    "geojson_fill_color": "#3388ff",
    "geojson_stroke_color": "#3388ff",
    "geojson_fill_opacity": "0.2",
    "collection_layers_enabled": false,
    "collection_layers_collection": "",
    "collection_layers_field": "",
    "collection_layers_limit": 50,
    "collection_layers_filter_field": "",
    "collection_layers_filter_value": "",
    "collection_layers_filter_value_source": "static",
    "collection_layers_filter_url_segment": 1
  },
  "settings": {
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

## GeoJSON Support

The widget supports rendering GeoJSON data (polygons, lines, points) as map overlays.

### Static Source

Upload a `.geojson` file and provide its URL in `geojson_file`. The map fetches and renders the features on load.

### Dynamic Source

Bind the widget to a collection field containing GeoJSON data. The entry can be resolved:
- **Static**: By a specific `entry_id`
- **URL**: From a URL path segment (e.g., segment 2 from `/regions/mazowieckie/` = `mazowieckie`)

## Collection GeoJSON Layers

When `collection_layers_enabled` is `true`, the widget loads all entries from the specified collection and renders each entry's GeoJSON field as a separate clickable map layer.

## Usage Example

```javascript
function renderOpenStreetMap(widget) {
  const { lat, lng, zoom, tile_style, show_marker, scroll_wheel_zoom, zoom_control, draggable } = widget.widget;

  const tileUrls = {
    standard: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
    light: 'https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}{r}.png',
    dark: 'https://{s}.basemaps.cartocdn.com/dark_all/{z}/{x}/{y}{r}.png'
  };

  const map = L.map('map', {
    center: [lat || 52.2297, lng || 21.0122],
    zoom: zoom || 14,
    scrollWheelZoom: !!scroll_wheel_zoom,
    zoomControl: zoom_control !== false,
    dragging: draggable !== false
  });

  L.tileLayer(tileUrls[tile_style] || tileUrls.standard, {
    attribution: '&copy; OpenStreetMap contributors'
  }).addTo(map);

  if (show_marker) {
    L.marker([lat, lng]).addTo(map);
  }
}
```

> **Note**: This widget requires [Leaflet.js](https://leafletjs.com/) library to be loaded on the page.
