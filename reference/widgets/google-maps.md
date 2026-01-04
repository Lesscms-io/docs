# Google Maps Widget

An embedded Google Maps widget with customizable location, zoom, and map type.

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
| `config.location_source` | string | `"address"` or `"coordinates"` |
| `config.address` | object | Multilingual address text |
| `config.lat` | number | Latitude (when using coordinates) |
| `config.lng` | number | Longitude (when using coordinates) |
| `config.zoom` | number | Zoom level (1-21) |
| `config.map_type` | string | Map type: `"roadmap"` or `"satellite"` |
| `config.show_marker` | boolean | Show marker on map |
| `settings` | object | Style settings (optional) |

> **Note**: The API does not expose the Google Maps API key for security reasons. Use the embed URL method or configure your own API key on the frontend.

## Example Response (Address)

```json
{
  "widget_type": "google-maps",
  "uuid": "maps-123",
  "config": {
    "location_source": "address",
    "address": {
      "en": "1600 Amphitheatre Parkway, Mountain View, CA",
      "pl": "1600 Amphitheatre Parkway, Mountain View, CA"
    },
    "lat": null,
    "lng": null,
    "zoom": 14,
    "map_type": "roadmap",
    "show_marker": true
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

## Example Response (Coordinates)

```json
{
  "widget_type": "google-maps",
  "uuid": "maps-456",
  "config": {
    "location_source": "coordinates",
    "address": {},
    "lat": 37.4224764,
    "lng": -122.0842499,
    "zoom": 15,
    "map_type": "satellite",
    "show_marker": true
  },
  "settings": {}
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
  const { location_source, address, lat, lng, zoom, show_marker } = widget.config;
  const height = widget.settings?.minHeight || 400;

  let query = '';
  if (location_source === 'coordinates' && lat && lng) {
    query = `${lat},${lng}`;
  } else {
    query = address?.[language] || address?.en || '';
  }

  if (!query) return '';

  const encodedQuery = encodeURIComponent(query);

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
