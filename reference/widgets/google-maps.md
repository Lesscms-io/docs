# Google Maps Widget

An embedded Google Maps widget with customizable address, zoom, and map type.

## Widget Type

```
google-maps
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"google-maps"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget data |
| `widget.location_source` | string | Location source: `"address"` or `"coordinates"` (default: `"address"`) |
| `widget.address` | object | Multilingual address text `{ "en": "...", "pl": "..." }` |
| `widget.lat` | number\|null | Latitude for coordinate-based location |
| `widget.lng` | number\|null | Longitude for coordinate-based location |
| `widget.zoom` | number | Zoom level (1-21, default: 14) |
| `widget.map_type` | string | Map type: `"roadmap"` or `"satellite"` |
| `widget.show_marker` | boolean | Show map marker at the location (default: true) |
| `widget.street_view_control` | boolean | Show Street View pegman control (default: false) |
| `widget.zoom_control` | boolean | Show zoom +/- buttons (default: true) |
| `widget.fullscreen_control` | boolean | Show fullscreen button (default: true) |
| `widget.map_type_control` | boolean | Show map/satellite toggle (default: false) |
| `widget.scroll_wheel` | boolean | Enable zoom via scroll wheel (default: false) |
| `widget.draggable` | boolean | Allow map panning by dragging (default: true) |
| `settings` | object | Style settings (optional) |

> **Note**: The API key is intentionally not exposed in the API response for security reasons. The frontend application should provide its own Google Maps API key for rendering.

## Example Response (Address Mode)

```json
{
  "widget_type": "google-maps",
  "uuid": "maps-123",
  "widget": {
    "location_source": "address",
    "address": {
      "en": "1600 Amphitheatre Parkway, Mountain View, CA",
      "pl": "1600 Amphitheatre Parkway, Mountain View, CA"
    },
    "lat": null,
    "lng": null,
    "zoom": 14,
    "map_type": "roadmap",
    "show_marker": true,
    "street_view_control": false,
    "zoom_control": true,
    "fullscreen_control": true,
    "map_type_control": false,
    "scroll_wheel": false,
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

## Example Response (Coordinates Mode)

```json
{
  "widget_type": "google-maps",
  "uuid": "maps-456",
  "widget": {
    "location_source": "coordinates",
    "address": {},
    "lat": 37.4220,
    "lng": -122.0841,
    "zoom": 16,
    "map_type": "satellite",
    "show_marker": true,
    "street_view_control": true,
    "zoom_control": true,
    "fullscreen_control": true,
    "map_type_control": true,
    "scroll_wheel": true,
    "draggable": true
  }
}
```

## Map Type Values

| Value | Description |
|-------|-------------|
| `roadmap` | Standard road map view |
| `satellite` | Satellite imagery view |

## Zoom Levels

| Range | Description |
|-------|-------------|
| 1-5 | World / Continent view |
| 6-10 | Country / Region view |
| 11-14 | City view |
| 15-17 | Street view |
| 18-21 | Building view |

## Usage Example

```javascript
function renderGoogleMaps(widget, language) {
  const { location_source, address, lat, lng, zoom, map_type, show_marker } = widget.widget;
  const height = widget.settings?.minHeight || 400;

  let query;
  if (location_source === 'coordinates' && lat && lng) {
    query = `${lat},${lng}`;
  } else {
    query = address?.[language] || address?.en || '';
  }

  if (!query) return '';

  const encodedQuery = encodeURIComponent(query);

  // Using embed URL (no API key required for basic embeds)
  return `
    <div class="google-maps-wrapper" style="width: 100%; height: ${height}px; overflow: hidden;">
      <iframe
        src="https://www.google.com/maps?q=${encodedQuery}&z=${zoom || 14}&output=embed"
        width="100%"
        height="100%"
        style="border: 0;"
        loading="lazy"
        referrerpolicy="no-referrer-when-downgrade"
      ></iframe>
    </div>
  `;
}
```
