# Icon Widget

A simple icon display widget with customizable size and color.

## Widget Type

```
icon
```

## Data Properties

| Property | Type | Global | Description |
|----------|------|--------|-------------|
| `icon` | string | Yes | Font Awesome icon class (e.g., "fa-solid fa-star") |
| `size` | string | Yes | Icon size: `24`, `32`, `48`, `64` (in pixels) |
| `color` | string | Yes | Icon color (hex code) |

## Example Response

```json
{
  "widget_type": "icon",
  "uuid": "icon-123",
  "data": {
    "icon": "fa-solid fa-star",
    "size": "48",
    "color": "#FFD700"
  },
  "settings": {
    "textAlign": "center",
    "marginTop": 10,
    "marginBottom": 10
  }
}
```

## Size Values

| Value | Description |
|-------|-------------|
| `24` | Small (24px) |
| `32` | Medium (32px) |
| `48` | Large (48px) |
| `64` | Extra large (64px) |

## Usage Example

```javascript
// Render icon widget
function renderIcon(widget) {
  const { icon, size, color } = widget.data;

  return `
    <i
      class="${icon}"
      style="font-size: ${size}px; color: ${color};"
    ></i>
  `;
}
```

## Font Awesome Integration

The icon field uses Font Awesome icons. Make sure to include Font Awesome in your frontend:

```html
<link
  rel="stylesheet"
  href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css"
>
```
