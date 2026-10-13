# Service Map Widget

An SVG map of the service area ("Mapa obszaru działania"): the country outline (dashed, lightly filled), city dots with names and coordinates, a larger headquarters marker with a pulsing ring, and lines drawn from the headquarters to every city when the map scrolls into view.

- City labels are real SVG text. The map also exposes an `aria-label` with all city names and a visually hidden list for screen readers.
- Labels are placed automatically (right, left, above, below) so that neighbouring cities (e.g. the Silesian cluster) do not overlap. A city can override its label position with `label_dx` / `label_dy` (px from the dot).
- Scales with the container (`viewBox 0 0 600 600`). Labels are larger on mobile (≤768px).
- `prefers-reduced-motion` disables the pulse and the line animation.

## Widget Type

```
service-map
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"service-map"` |
| `uuid` | string | Unique widget identifier |
| `widget.config` | object | Map configuration |
| `widget.config.country` | string | Country outline (`pl`, the only one for now) |
| `widget.config.animate_lines` | boolean | Draw lines when the map enters the viewport (default `true`) |
| `widget.config.show_coords` | boolean | Show coordinates under city names (default `true`) |
| `widget.config.show_lines` | boolean | Draw lines from the headquarters to cities (default `true`) |
| `widget.caption` | object | Caption in the top-left corner |
| `widget.caption.text` | object | Multilingual caption, e.g. `Obszar działania · siedziba: Warszawa` |
| `widget.caption.color` | string\|null | Caption color (default `var:primary`) |
| `widget.caption.text_style` | string\|null | Project text style code (`lcms-ts-<code>` class on root) |
| `widget.map` | object | Map colors |
| `widget.map.outline_color` | string\|null | Country outline color |
| `widget.map.fill` | string\|null | Country fill color |
| `widget.map.background` | string\|null | Map background color |
| `widget.map.border_color` | string\|null | Frame color (no frame when `null`) |
| `widget.dot` | object | City markers |
| `widget.dot.color` | string\|null | City dot color (default: current text color) |
| `widget.dot.hq_color` | string\|null | Headquarters color (default `var:primary`) |
| `widget.dot.size` | number | City dot radius in map units (default `4.5`; headquarters is about 1.8x) |
| `widget.line` | object | Lines from the headquarters |
| `widget.line.color` | string\|null | Line color (default `var:primary`) |
| `widget.line.width` | number | Line width (default `1.5`) |
| `widget.label` | object | City labels |
| `widget.label.color` | string\|null | Name color |
| `widget.label.coords_color` | string\|null | Coordinates color |
| `widget.items` | array | Cities (points without numeric `lat`/`lon` are skipped) |
| `widget.items[].name` | object | Multilingual city name |
| `widget.items[].lat` | number | Latitude (decimal degrees) |
| `widget.items[].lon` | number | Longitude (decimal degrees) |
| `widget.items[].is_hq` | boolean | Headquarters marker (use one city) |
| `widget.items[].show_coords` | boolean | Show this city's coordinates (default `true`) |
| `widget.items[].label_dx` | number\|null | Manual label X offset in px (`null` = automatic) |
| `widget.items[].label_dy` | number\|null | Manual label Y offset in px (`null` = automatic) |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "service-map",
  "uuid": "sm-001",
  "widget": {
    "config": { "country": "pl", "animate_lines": true, "show_coords": true, "show_lines": true },
    "caption": { "text": { "pl": "Obszar działania · siedziba: Warszawa" }, "color": "var:primary", "text_style": null },
    "map": { "outline_color": null, "fill": null, "background": null, "border_color": null },
    "dot": { "color": null, "hq_color": "var:primary", "size": 4.5 },
    "line": { "color": "var:primary", "width": 1.5 },
    "label": { "color": null, "coords_color": null },
    "items": [
      { "name": { "pl": "Warszawa" }, "lat": 52.23, "lon": 21.01, "is_hq": true, "show_coords": true, "label_dx": null, "label_dy": null },
      { "name": { "pl": "Kraków" }, "lat": 50.06, "lon": 19.94, "is_hq": false, "show_coords": true, "label_dx": null, "label_dy": null }
    ]
  },
  "settings": {}
}
```
