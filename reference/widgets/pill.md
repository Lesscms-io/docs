# Pill Widget

A small label/badge element with configurable style, commonly used for tags, categories, or status indicators.

## Widget Type

```
pill
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"pill"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget data |
| `widget.text` | object | Text element-group |
| `widget.text.content` | object | Multilingual pill text |
| `widget.text.color` | string\|null | Text color (color variable or hex, default: `"var:white"`) |
| `widget.text.color:hover` | string\|null | Text color on hover |
| `widget.config` | object | Config element-group |
| `widget.config.variant` | string | `"filled"` or `"outline"` (default: `"filled"`) |
| `widget.config.size` | string | `"sm"`, `"md"`, or `"lg"` (default: `"md"`) |
| `widget.config.uppercase` | boolean | Whether text is uppercase (default: `true`) |
| `settings` | object | [Shared style settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "pill",
  "uuid": "pill-123",
  "widget": {
    "text": {
      "content": {
        "en": "New",
        "pl": "Nowo\u015b\u0107"
      },
      "color": "var:white",
      "color:hover": null
    },
    "config": {
      "variant": "filled",
      "size": "md",
      "uppercase": true
    }
  },
  "settings": {
    "background_color": "var:primary",
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

## Variant Values

| Value | Description |
|-------|-------------|
| `filled` | Solid background with text color |
| `outline` | Transparent background with border |

## Size Values

| Value | Description |
|-------|-------------|
| `sm` | Small pill |
| `md` | Medium pill (default) |
| `lg` | Large pill |

## Usage Example

```javascript
function renderPill(widget, language) {
  const { text, config } = widget.widget;
  const label = text?.content?.[language] || text?.content?.en || '';
  const variant = config?.variant || 'filled';
  const size = config?.size || 'md';
  const uppercase = config?.uppercase !== false;

  const color = text?.color || '#fff';
  const bgColor = widget.settings?.background_color || '#50a5f1';

  const style = variant === 'outline'
    ? `border: 1px solid ${bgColor}; color: ${bgColor}; background: transparent;`
    : `background: ${bgColor}; color: ${color};`;

  return `<span class="pill pill--${size}" style="${style}${uppercase ? ' text-transform: uppercase;' : ''}">${label}</span>`;
}
```
