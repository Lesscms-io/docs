# Divider Widget

A horizontal line separator with customizable style, color, and thickness.

## Widget Type

```
divider
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"divider"` |
| `uuid` | string | Unique widget identifier |
| `config` | object | Widget configuration |
| `config.style` | string | Line style: `"solid"`, `"dashed"`, `"dotted"` |
| `config.color` | string | Line color (hex code) |
| `config.thickness` | number | Line thickness in pixels |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "divider",
  "uuid": "divider-123",
  "config": {
    "style": "solid",
    "color": "#e9ecef",
    "thickness": 1
  },
  "settings": {
    "marginTop": 20,
    "marginBottom": 20,
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
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
function renderDivider(widget) {
  const { style, color, thickness } = widget.config;

  return `<hr style="border: none; border-top: ${thickness}px ${style} ${color}; margin: 0;">`;
}
```
