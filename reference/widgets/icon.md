# Icon Widget

A simple icon display widget.

## Widget Type

```
icon
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"icon"` |
| `uuid` | string | Unique widget identifier |
| `config` | object | Widget configuration |
| `config.icon` | string | Icon class (e.g., `"bx bx-phone"`) |
| `config.size` | string | Icon size in pixels (e.g., `"48"`) |
| `config.color` | string | Icon color (hex, e.g., `"#333333"`) |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "icon",
  "uuid": "icon-123",
  "config": {
    "icon": "bx bx-phone",
    "size": "48",
    "color": "#50a5f1"
  },
  "settings": {
    "horizontalAlign": "center",
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Icon Libraries

The system supports various icon libraries:

- **Boxicons** - `bx bx-*`, `bx bxs-*` (solid), `bx bxl-*` (logos)
- **Font Awesome** - `fa fa-*`, `fas fa-*`, `fab fa-*`
- **Custom** - Any CSS class that renders an icon

## Usage Example

```javascript
function renderIcon(widget) {
  const { icon, size, color } = widget.config;

  return `<i class="${icon}" style="font-size: ${size}px; color: ${color};"></i>`;
}
```
