# Divider Widget

A horizontal line separator with customizable style, color, and width.

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
| `config.width` | string | Line width in pixels: `"1"`, `"2"`, `"3"` |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "divider",
  "uuid": "divider-123",
  "config": {
    "style": "solid",
    "color": "#e9ecef",
    "width": "1"
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

## Width Values

| Value | Description |
|-------|-------------|
| `"1"` | 1px line |
| `"2"` | 2px line |
| `"3"` | 3px line |

## Usage Example

```javascript
function renderDivider(widget) {
  const { style, color, width } = widget.config;

  return `<hr style="border: none; border-top: ${width}px ${style} ${color}; margin: 0;">`;
}
```
