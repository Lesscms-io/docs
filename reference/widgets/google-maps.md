# Google Maps Widget

An embedded Google Maps widget with customizable location, zoom, and map type.

## Widget Type

```
google-maps
```

## Data Properties

| Property | Type | Global | Description |
|----------|------|--------|-------------|
| `api_key` | string | Yes | Google Maps API key |
| `address` | object | No | Location address (multilingual) |
| `zoom` | number | Yes | Zoom level (1-21) |
| `map_type` | string | Yes | Map type: `roadmap` or `satellite` |

## Example Response

```json
{
  "widget_type": "google-maps",
  "uuid": "maps-123",
  "data": {
    "api_key": "AIzaSyB...",
    "address": {
      "en": "1600 Amphitheatre Parkway, Mountain View, CA",
      "pl": "1600 Amphitheatre Parkway, Mountain View, CA"
    },
    "zoom": 15,
    "map_type": "roadmap"
  },
  "settings": {
    "height": 400,
    "width": "100%",
    "borderRadius": 8
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

## Usage Example (Embed API)

```javascript
// Render Google Maps using Embed API (no API key required for basic embed)
function renderGoogleMapsEmbed(widget, language) {
  const { address, zoom } = widget.data;
  const settings = widget.settings || {};

  const addressText = address?.[language] || address?.en || '';
  if (!addressText) return '';

  const encodedAddress = encodeURIComponent(addressText);
  const height = settings.height || 400;

  return `
    <div class="google-maps-wrapper" style="
      width: 100%;
      height: ${height}px;
      border-radius: ${settings.borderRadius || 0}px;
      overflow: hidden;
    ">
      <iframe
        src="https://www.google.com/maps?q=${encodedAddress}&z=${zoom || 14}&output=embed"
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

## Usage Example (Maps JavaScript API)

```javascript
// Render Google Maps using JavaScript API (requires API key)
function renderGoogleMapsJS(widget, language) {
  const { api_key, address, zoom, map_type } = widget.data;
  const settings = widget.settings || {};

  const addressText = address?.[language] || address?.en || '';
  const mapId = `map-${widget.uuid}`;

  return `
    <div id="${mapId}" style="
      width: 100%;
      height: ${settings.height || 400}px;
      border-radius: ${settings.borderRadius || 0}px;
    "></div>
    <script>
      (function() {
        const geocoder = new google.maps.Geocoder();
        geocoder.geocode({ address: "${addressText}" }, (results, status) => {
          if (status === 'OK') {
            const map = new google.maps.Map(document.getElementById('${mapId}'), {
              zoom: ${zoom || 14},
              center: results[0].geometry.location,
              mapTypeId: '${map_type || 'roadmap'}'
            });
            new google.maps.Marker({
              map: map,
              position: results[0].geometry.location
            });
          }
        });
      })();
    </script>
  `;
}
```

## Security Note

> **Important**: Never expose your API key in client-side code without proper restrictions. Configure your API key in Google Cloud Console to:
> - Restrict by HTTP referrers (your domain only)
> - Limit to specific APIs (Maps JavaScript API, Geocoding API)
