# Icon List Widget

A multi-item widget displaying items with icons and text.

## Widget Type

```
icon-list
```

## Multi-Item Support

This widget supports the multi-item pattern. When multiple items exist, the response includes `multi_item: true` with an `items` array.

## Response Structure (Single Item)

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"icon-list"` |
| `uuid` | string | Unique widget identifier |
| `content` | object | Widget content |
| `content.text` | string | Item text (localized) |
| `config` | object | Widget configuration |
| `config.icon` | string\|null | FontAwesome icon class |
| `config.icon_color` | string\|null | Icon color |
| `config.text_size` | string | Text size: `"sm"`, `"md"`, `"lg"` |
| `config.item_bg_color` | string\|null | Item background color |
| `config.icon_size` | string | Icon size: `"sm"`, `"md"`, `"lg"` |
| `settings` | object | Style settings (optional) |

## Example Response (Multi-Item)

```json
{
  "widget_type": "icon-list",
  "uuid": "icon-list-123",
  "multi_item": true,
  "multi_columns": 1,
  "multi_gap": 16,
  "items": [
    {
      "widget_type": "icon-list",
      "content": { "text": "Free shipping worldwide" },
      "config": {
        "icon": "fa-solid fa-truck",
        "icon_color": "#50a5f1",
        "text_size": "md",
        "item_bg_color": null,
        "icon_size": "md"
      }
    },
    {
      "widget_type": "icon-list",
      "content": { "text": "30-day money back guarantee" },
      "config": {
        "icon": "fa-solid fa-shield-check",
        "icon_color": "#50a5f1",
        "text_size": "md",
        "item_bg_color": null,
        "icon_size": "md"
      }
    }
  ]
}
```

## Usage Example

```javascript
function renderIconList(widget) {
  if (widget.multi_item) {
    const cols = widget.multi_columns || 1;
    const gap = widget.multi_gap || 16;
    const items = widget.items.map(item => renderIconListItem(item)).join('');
    return `<div style="display: grid; grid-template-columns: repeat(${cols}, 1fr); gap: ${gap}px">${items}</div>`;
  }
  return renderIconListItem(widget);
}

function renderIconListItem(widget) {
  const { text } = widget.content;
  const { icon, icon_color, icon_size, item_bg_color } = widget.config;

  const sizeMap = { sm: '16px', md: '24px', lg: '32px' };

  return `
    <div class="icon-list-item" style="${item_bg_color ? `background:${item_bg_color}; padding:10px 14px; border-radius:6px;` : ''}">
      <i class="${icon || 'fa-solid fa-circle'}" style="color: ${icon_color || 'inherit'}; font-size: ${sizeMap[icon_size] || '24px'}"></i>
      <span>${text}</span>
    </div>
  `;
}
```
