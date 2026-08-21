# Icon Widget

A standalone icon — Font Awesome class or custom SVG, with size, colors and hover states. Make it clickable via the shared widget link settings. Wrappable for multi-column layouts.

## Widget Type

```
icon
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"icon"` |
| `uuid` | string | Unique widget identifier |
| `widget.icon` | object | Icon element |
| `widget.icon.icon` | string\|null | Icon class (e.g. `fa-solid fa-star`) or inline SVG prefixed with `svg:` |
| `widget.icon.size` | number | Icon size (px), default 48 |
| `widget.icon.padding` | number | Icon padding (px), default 12 |
| `widget.icon.border_radius` | number | Background border radius (px), default 8 |
| `widget.icon.color` | string\|null | Icon color (`var:` color variables supported) |
| `widget.icon.color:hover` | string\|null | Icon color on hover |
| `widget.icon.background` | string\|null | Icon background color |
| `widget.icon.background:hover` | string\|null | Icon background on hover |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Wrappable

Yes — can be wrapped in a multi-column grid layout.

## Linking

The widget has no dedicated link fields — use the shared link settings (`settings.link`) to make the icon clickable, same as any other widget.

## Example Response

```json
{
  "widget_type": "icon",
  "uuid": "ic-001",
  "widget": {
    "icon": {
      "icon": "fa-solid fa-camera",
      "size": 64,
      "padding": 12,
      "border_radius": 8,
      "color": "var:primary",
      "color:hover": "var:secondary",
      "background": null,
      "background:hover": null
    }
  },
  "settings": {}
}
```

## Rendering Notes

- `icon` values starting with `svg:` contain inline SVG markup after the prefix — render with `fill: currentColor` so `color` applies.
- Icon box (`width`/`height`) is `size + 2 × padding`; background and border radius apply to that box.
- Hover colors transition over 200ms.
