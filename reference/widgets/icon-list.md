# Icon List Widget

A multi-item widget displaying items with icons and text.

## Widget Type

```
icon-list
```

## Multi-Item Support

This widget supports the multi-item pattern. When multiple items exist, the response includes `multi_item: true` with an `items` array.

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"icon-list"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget data |
| `widget.icon` | object | Icon element group |
| `widget.icon.icon` | string | FontAwesome icon class (default: `"fa-solid fa-check"`) |
| `widget.icon.size` | string | Icon size: `"sm"`, `"md"`, `"lg"` (default: `"md"`) |
| `widget.icon.color` | string\|null | Icon color |
| `widget.icon.color:hover` | string\|null | Icon color on hover |
| `widget.text` | object | Text element group |
| `widget.text.html` | object | Multilingual item text (HTML) |
| `widget.text.size` | string | Text size: `"sm"`, `"md"`, `"lg"` (default: `"md"`) |
| `widget.item_style` | object | Item styling group |
| `widget.item_style.background` | string\|null | Item background color |
| `widget.item_style.background:hover` | string\|null | Item background color on hover |
| `settings` | object | Style settings (padding, background, border, hover transforms) |

## Example Response

```json
{
  "widget_type": "icon-list",
  "uuid": "icon-list-123",
  "widget": {
    "icon": {
      "icon": "fa-solid fa-check",
      "size": "md",
      "color": "var:primary",
      "color:hover": null
    },
    "text": {
      "html": {
        "en": "Free shipping worldwide",
        "pl": "Darmowa dostawa na caly swiat"
      },
      "size": "md"
    },
    "item_style": {
      "background": null,
      "background:hover": null
    }
  },
  "settings": {
    "padding_top": 12,
    "padding_bottom": 12,
    "padding_left": 0,
    "padding_right": 0
  }
}
```

## Usage Example

```javascript
function renderIconList(widget, language) {
  const { icon, text, item_style } = widget.widget;

  const sizeMap = { sm: '16px', md: '24px', lg: '32px' };
  const textSizeMap = { sm: '0.875rem', md: '1rem', lg: '1.125rem' };

  const iconClass = icon?.icon || 'fa-solid fa-circle';
  const iconColor = icon?.color || 'inherit';
  const iconSize = sizeMap[icon?.size || 'md'] || '24px';
  const textSize = textSizeMap[text?.size || 'md'] || '1rem';
  const itemText = text?.html?.[language] || text?.html?.en || '';
  const bgColor = item_style?.background;

  return `
    <div class="icon-list-item" style="${bgColor ? `background:${bgColor}; padding:10px 14px; border-radius:6px;` : ''}">
      <i class="${iconClass}" style="color: ${iconColor}; font-size: ${iconSize}"></i>
      <span style="font-size: ${textSize}">${itemText}</span>
    </div>
  `;
}
```
