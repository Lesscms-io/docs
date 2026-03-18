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
| `widget.line` | object | Line element-group |
| `widget.line.line_style` | string | Line style: `"solid"`, `"dashed"`, `"dotted"` |
| `widget.line.color` | string | Line color (color variable or hex, default: `"var:border"`) |
| `widget.line.color:hover` | string\|null | Line color on hover |
| `widget.line.width` | string | Line thickness in pixels: `"1"`, `"2"`, `"3"` |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "divider",
  "uuid": "divider-123",
  "widget": {
    "line": {
      "line_style": "solid",
      "color": "var:border",
      "color:hover": null,
      "width": "1"
    }
  },
  "settings": {
    "padding_top": 8,
    "padding_bottom": 8,
    "padding_left": 0,
    "padding_right": 0,
    "transition_duration": 200,
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Line Style Values

| Value | Description |
|-------|-------------|
| `solid` | Continuous line |
| `dashed` | Dashed line |
| `dotted` | Dotted line |

## Width Values

| Value | Description |
|-------|-------------|
| `"1"` | 1px line (default) |
| `"2"` | 2px line |
| `"3"` | 3px line |

## Usage Example

```javascript
function renderDivider(widget) {
  const { line } = widget.widget;
  const style = line.line_style || 'solid';
  const color = line.color || '#e9ecef';
  const width = line.width || '1';

  return `<hr style="border: none; border-top: ${width}px ${style} ${color}; margin: 0;">`;
}
```
