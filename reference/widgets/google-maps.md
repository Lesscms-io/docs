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
| `widget.api_key` | string | Google Maps API key (intentionally excluded from API response for security) |
| `widget.address` | object | Multilingual address text `{ "en": "...", "pl": "..." }` |
| `widget.zoom` | number | Zoom level (1-21, default: 14) |
| `widget.map_type` | string | Map type: `"roadmap"` or `"satellite"` |
| `widget.street_view_control` | boolean | Show Street View pegman control (default: false) |
| `widget.zoom_control` | boolean | Show zoom +/- buttons (default: true) |
| `widget.fullscreen_control` | boolean | Show fullscreen button (default: true) |
| `widget.map_type_control` | boolean | Show map/satellite toggle (default: false) |
| `widget.scroll_wheel` | boolean | Enable zoom via scroll wheel (default: false) |
| `widget.draggable` | boolean | Allow map panning by dragging (default: true) |
| `settings` | object | Style settings (optional) |

> **Note**: The `api_key` field is intentionally excluded from the API response for security. The frontend application should provide its own Google Maps API key.

## Example Response

```json
{
  "widget_type": "google-maps",
  "uuid": "maps-123",
  "widget": {
    "address": {
      "en": "1600 Amphitheatre Parkway, Mountain View, CA",
      "pl": "1600 Amphitheatre Parkway, Mountain View, CA"
    },
    "zoom": 14,
    "map_type": "roadmap",
    "street_view_control": false,
    "zoom_control": true,
    "fullscreen_control": true,
    "map_type_control": false,
    "scroll_wheel": false,
    "draggable": true
  },
  "settings": {
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
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
  const { address, zoom, map_type } = widget.widget;

  const query = address?.[language] || address?.en || '';
  if (!query) return '';

  const encodedQuery = encodeURIComponent(query);

  return `
    <div class="google-maps-wrapper" style="width: 100%; height: 400px; overflow: hidden;">
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
