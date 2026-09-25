# Hotspots Widget (premium)

Markers pinned on an image — each opens a tooltip with a title, text,
optional image and link. Product callouts, floor plans, "shop the look".
Requires the `premium_widgets` plan feature.

## Widget Type

```
hotspots
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"hotspots"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget properties |
| `widget.image` | object | Base image |
| `widget.image.src` | string | Image URL |
| `widget.image.alt` | object | Multilingual alt text |
| `widget.image.radius` | number | Corner radius in px (default `12`) |
| `widget.items` | array | Markers |
| `widget.items[].x` | number | Horizontal position, `0`–`100` % of the image width |
| `widget.items[].y` | number | Vertical position, `0`–`100` % of the image height |
| `widget.items[].title` | object | Multilingual tooltip title |
| `widget.items[].text` | object | Multilingual tooltip text |
| `widget.items[].image` | string | Tooltip image URL |
| `widget.items[].icon` | string | Font Awesome class (used with `marker.style = "icon"`) |
| `widget.items[].url` | string | Resolved link URL (empty = no link) |
| `widget.items[].link_type` | string | `"custom"`, `"page"`, `"entry"`, `"route"` |
| `widget.items[].page_id` | string\|null | Page UUID for page links |
| `widget.items[].collection_code` | string\|null | Collection code for entry links |
| `widget.items[].entry_id` | string\|null | Entry UUID for entry links |
| `widget.items[].route_uuid` | string\|null | Route UUID for route links |
| `widget.items[].target_blank` | boolean | Open the link in a new tab |
| `widget.marker` | object | Marker look |
| `widget.marker.style` | string | `"dot"`, `"number"` (1, 2, 3…), `"icon"` (per-item icon) |
| `widget.marker.size` | number | Marker diameter in px (default `28`) |
| `widget.marker.color` | string | Icon / number color |
| `widget.marker.background` | string | Marker background |
| `widget.marker.background:hover` | string\|null | Marker background on hover / open |
| `widget.marker.pulse` | boolean | Pulsing halo animation (default `true`) |
| `widget.tooltip` | object | Tooltip behaviour and look |
| `widget.tooltip.trigger` | string | `"hover"`, `"click"`, `"always"` (all tooltips open) |
| `widget.tooltip.position` | string | `"auto"` (by marker position), `"top"`, `"bottom"`, `"left"`, `"right"` |
| `widget.tooltip.background` | string | Tooltip background |
| `widget.tooltip.color` | string | Tooltip text color |
| `widget.tooltip.width` | number | Tooltip width in px (default `240`) |
| `widget.tooltip.radius` | number | Tooltip corner radius in px (default `10`) |
| `settings` | object | Style settings (padding, background, border, hover transforms) |

## Example Response

```json
{
  "widget_type": "hotspots",
  "uuid": "hotspots-123",
  "widget": {
    "image": { "src": "https://img.lesscms.io/…/living-room.jpg", "alt": { "en": "Living room", "pl": "Salon" }, "radius": 12 },
    "items": [
      {
        "x": 32.5, "y": 61,
        "title": { "en": "Oak coffee table", "pl": "Dębowy stolik" },
        "text": { "en": "Solid oak, hand oiled.", "pl": "Lity dąb, olejowany ręcznie." },
        "image": "", "icon": "fa-solid fa-plus",
        "url": "/products/oak-table", "link_type": "entry", "page_id": null, "collection_code": "products", "entry_id": "…", "route_uuid": null, "target_blank": false
      }
    ],
    "marker": { "style": "dot", "size": 28, "color": "#ffffff", "background": "var:primary", "background:hover": null, "pulse": true },
    "tooltip": { "trigger": "hover", "position": "auto", "background": "#ffffff", "color": "var:dark", "width": 240, "radius": 10 }
  },
  "settings": {}
}
```

## Rendering Notes

- Position each marker absolutely at `left: x%; top: y%` with `transform: translate(-50%, -50%)` so it stays put when the image scales.
- `position: "auto"`: markers in the top third open downwards, bottom third upwards, otherwise sideways away from the nearer edge.
- Markers should be `<button>`s (keyboard + `aria-expanded`); close open tooltips on outside click.
