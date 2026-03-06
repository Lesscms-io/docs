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
| `settings` | object | Style settings (optional) |

## Example Response

### Basic (marker only)

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
    "geojson_fill_color": "",
    "geojson_stroke_color": "",
    "geojson_fill_opacity": ""
  },
  "settings": {
    "minHeight": 400
  }
}
```

### With GeoJSON file (static)

```json
{
  "widget_type": "openstreetmap",
  "uuid": "osm-456",
  "widget": {
    "lat": 52.2297,
    "lng": 21.0122,
    "zoom": 10,
    "tile_style": "light",
    "show_marker": false,
    "scroll_wheel_zoom": true,
    "zoom_control": true,
    "draggable": true,
    "geojson_source": "static",
    "geojson_file": "https://example.com/files/districts.geojson",
    "collection_code": "",
    "entry_source": "static",
    "entry_id": "",
    "entry_url_segment": 1,
    "geojson_field_code": "",
    "geojson_fill_color": "#ff6600",
    "geojson_stroke_color": "#cc3300",
    "geojson_fill_opacity": "0.3"
  },
  "settings": {
    "minHeight": 500
  }
}
```

### With dynamic GeoJSON (from collection)

```json
{
  "widget_type": "openstreetmap",
  "uuid": "osm-789",
  "widget": {
    "lat": 52.2297,
    "lng": 21.0122,
    "zoom": 12,
    "tile_style": "standard",
    "show_marker": true,
    "scroll_wheel_zoom": false,
    "zoom_control": true,
    "draggable": true,
    "geojson_source": "dynamic",
    "geojson_file": "",
    "collection_code": "regions",
    "entry_source": "url",
    "entry_id": "",
    "entry_url_segment": 2,
    "geojson_field_code": "boundary_data",
    "geojson_fill_color": "#3388ff",
    "geojson_stroke_color": "#3388ff",
    "geojson_fill_opacity": "0.2"
  },
  "settings": {
    "minHeight": 400
  }
}
```

### With collection GeoJSON layers

```json
{
  "widget_type": "openstreetmap",
  "uuid": "osm-layers",
  "widget": {
    "lat": 52.2297,
    "lng": 21.0122,
    "zoom": 10,
    "tile_style": "light",
    "show_marker": false,
    "scroll_wheel_zoom": true,
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
    "collection_layers_enabled": true,
    "collection_layers_collection": "regions",
    "collection_layers_field": "geojson_file"
  },
  "settings": {
    "minHeight": 500
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

The widget supports rendering GeoJSON data (polygons, lines, points) as map overlays. GeoJSON data can come from two sources:

### Static Source

Upload a `.geojson` file to the file manager and select it. The file URL is stored in `geojson_file`. When the map loads, it fetches the file and renders the features.

### Dynamic Source

Bind the widget to a collection field that contains GeoJSON data. The entry can be resolved:
- **Static**: By a specific `entry_id`
- **URL**: From a URL path segment (e.g., segment 2 from `/regions/mazowieckie/` → `mazowieckie`)

### Rendering Behavior

- GeoJSON features are rendered with customizable fill color, stroke color, and opacity
- When GeoJSON data is loaded, the map automatically adjusts its viewport (`fitBounds`) to show all features
- Point features are rendered as circle markers
- If no GeoJSON data is configured, the map works as before (marker only)

## Collection GeoJSON Layers

When `collection_layers_enabled` is `true`, the widget loads all entries from the specified collection and renders each entry's GeoJSON field as a separate map layer.

- **`collection_layers_collection`**: The collection code to fetch entries from
- **`collection_layers_field`**: The field code in each entry that contains GeoJSON data (can be a file URL or inline GeoJSON)

Each GeoJSON area is clickable and navigates to the entry's URL (as returned by `entry.metadata.url`). Hovering over an area highlights it with increased opacity. The map automatically fits its viewport to include all collection layers.

This feature works alongside the single GeoJSON source — you can have both a static/dynamic GeoJSON layer and collection layers on the same map.

## Usage Example

```javascript
function renderOpenStreetMap(widget) {
  const {
    lat, lng, zoom,
    tile_style,
    show_marker,
    scroll_wheel_zoom,
    zoom_control,
    draggable,
    geojson_source,
    geojson_file,
    geojson_fill_color,
    geojson_stroke_color,
    geojson_fill_opacity
  } = widget.widget;

  const height = widget.settings?.minHeight || 400;

  const tileUrls = {
    standard: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
    light: 'https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}{r}.png',
    dark: 'https://{s}.basemaps.cartocdn.com/dark_all/{z}/{x}/{y}{r}.png'
  };

  const mapId = `map-${widget.uuid}`;

  // Initialize map
  const map = L.map(mapId, {
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

  // Load GeoJSON if configured
  if (geojson_source === 'static' && geojson_file) {
    fetch(geojson_file)
      .then(r => r.json())
      .then(data => {
        const layer = L.geoJSON(data, {
          style: {
            fillColor: geojson_fill_color || '#3388ff',
            color: geojson_stroke_color || '#3388ff',
            fillOpacity: parseFloat(geojson_fill_opacity) || 0.2,
            weight: 2
          }
        }).addTo(map);
        map.fitBounds(layer.getBounds());
      });
  }
}
```

> **Note**: This widget requires [Leaflet.js](https://leafletjs.com/) library to be loaded on the page. Include `leaflet.css` and `leaflet.js` in your HTML head.
