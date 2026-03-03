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
| `widget.variant` | string | `"filled"` or `"outline"` |
| `widget.size` | string | `"sm"`, `"md"`, or `"lg"` |
| `widget.background_color` | string | Background color |
| `widget.text_color` | string | Text color |
| `widget.uppercase` | boolean | Whether text is uppercase (default: true) |
| `widget.text` | object | Multilingual pill text |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "pill",
  "uuid": "pill-123",
  "widget": {
    "variant": "filled",
    "size": "md",
    "background_color": "#50a5f1",
    "text_color": "#ffffff",
    "uppercase": true,
    "text": {
      "en": "New",
      "pl": "Nowość"
    }
  },
  "settings": {}
}
```

## Multi-Item Support

The pill widget supports displaying multiple pills in a grid. When multiple items are configured, the API returns a multi-item structure:

```json
{
  "widget_type": "pill",
  "uuid": "pill-multi",
  "multi_item": true,
  "multi_columns": 3,
  "multi_gap": 16,
  "items": [
    {
      "widget_type": "pill",
      "widget": { "variant": "filled", "size": "md", "background_color": "#50a5f1", "text_color": "#fff", "uppercase": true, "text": { "en": "Design" } }
    },
    {
      "widget_type": "pill",
      "widget": { "variant": "filled", "size": "md", "background_color": "#50a5f1", "text_color": "#fff", "uppercase": true, "text": { "en": "Development" } }
    },
    {
      "widget_type": "pill",
      "widget": { "variant": "filled", "size": "md", "background_color": "#50a5f1", "text_color": "#fff", "uppercase": true, "text": { "en": "Marketing" } }
    }
  ],
  "settings": {}
}
```

Per-item fields (`text`) are unique to each item. Shared fields (`variant`, `size`, `background_color`, `text_color`, `uppercase`) are the same across all items.

## Usage Example

```javascript
function renderPill(widget, language) {
  const { variant, size, background_color, text_color, uppercase } = widget.widget;
  const text = widget.widget?.text?.[language] || widget.widget?.text?.en || '';

  const style = variant === 'outline'
    ? `border: 1px solid ${background_color}; color: ${background_color}; background: transparent;`
    : `background: ${background_color || '#50a5f1'}; color: ${text_color || '#fff'};`;

  return `<span class="pill pill--${size}" style="${style}${uppercase ? ' text-transform: uppercase;' : ''}">${text}</span>`;
}
```
