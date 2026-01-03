# Divider Widget

A horizontal line separator with customizable style, color, and width.

## Widget Type

```
divider
```

## Data Properties

| Property | Type | Global | Description |
|----------|------|--------|-------------|
| `style` | string | Yes | Line style: `solid`, `dashed`, `dotted` |
| `color` | string | Yes | Line color (hex code) |
| `width` | string | Yes | Line thickness: `1`, `2`, `3` (in pixels) |

## Example Response

```json
{
  "widget_type": "divider",
  "uuid": "divider-123",
  "data": {
    "style": "solid",
    "color": "#E0E0E0",
    "width": "1"
  },
  "settings": {
    "marginTop": 20,
    "marginBottom": 20
  }
}
```

## Style Values

| Value | Description |
|-------|-------------|
| `solid` | Continuous line |
| `dashed` | Dashed line |
| `dotted` | Dotted line |

## Usage Example

```javascript
// Render divider widget
function renderDivider(widget) {
  const { style, color, width } = widget.data;

  return `
    <hr style="
      border: none;
      border-top: ${width}px ${style} ${color};
      margin: 0;
    ">
  `;
}
```
