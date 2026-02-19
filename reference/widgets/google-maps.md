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
| `config` | object | Widget configuration |
| `config.api_key` | string | Google Maps API key |
| `config.address` | object | Multilingual address text `{ "en": "...", "pl": "..." }` |
| `config.zoom` | number | Zoom level (1-21, default: 14) |
| `config.map_type` | string | Map type: `"roadmap"` or `"satellite"` |
| `settings` | object | Style settings (optional) |

> **Note**: The `api_key` is configured per widget in the CMS. For security, consider proxying map requests through your backend rather than exposing the key in client-side code.

## Example Response

```json
{
  "widget_type": "google-maps",
  "uuid": "maps-123",
  "config": {
    "api_key": "AIzaSy...",
    "address": {
      "en": "1600 Amphitheatre Parkway, Mountain View, CA",
      "pl": "1600 Amphitheatre Parkway, Mountain View, CA"
    },
    "zoom": 14,
    "map_type": "roadmap"
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
  const { api_key, address, zoom, map_type } = widget.config;
  const height = widget.settings?.minHeight || 400;

  const query = address?.[language] || address?.en || '';
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
