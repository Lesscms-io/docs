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
| `widget` | object | Widget properties |
| `widget.style` | string | Line style: `"solid"`, `"dashed"`, `"dotted"` |
| `widget.color` | string | Line color (hex code, default: `"#e9ecef"`) |
| `widget.thickness` | number | Line thickness in pixels (default: 1) |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "divider",
  "uuid": "divider-123",
  "widget": {
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

## Thickness Values

| Value | Description |
|-------|-------------|
| `1` | 1px line (default) |
| `2` | 2px line |
| `3` | 3px line |

## Usage Example

```javascript
function renderDivider(widget) {
  const { style, color, thickness } = widget.widget;

  return `<hr style="border: none; border-top: ${thickness}px ${style} ${color}; margin: 0;">`;
}
```
