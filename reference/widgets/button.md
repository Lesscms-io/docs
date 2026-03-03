# Button Widget

A button widget with multilingual text and configurable link target.

## Widget Type

```
button
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"button"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget properties |
| `widget.link_type` | string | `"custom"`, `"page"`, or `"entry"` |
| `widget.style` | string | Button style: `"primary"`, `"secondary"`, `"outline"` |
| `widget.size` | string | Button size: `"sm"`, `"md"`, `"lg"` |
| `widget.target_blank` | boolean | Open in new tab |
| `widget.border_radius` | string | Button border radius preset (`"none"`, `"sm"`, `"md"`, `"lg"`, `"full"`) |
| `widget.padding` | string\|null | Custom button padding CSS value |
| `widget.icon` | string | Icon class (e.g., `"fa-solid fa-arrow-right"`) |
| `widget.icon_position` | string | Icon position relative to text |
| `widget.text` | object | Multilingual button text |
| `widget.url` | string | Resolved URL |
| `widget.page_uuid` | string\|null | Page UUID (for `page` links) |
| `widget.page_code` | string\|null | Page code (for `page` links) |
| `widget.route_uuid` | string\|null | Route UUID for URL resolution (for `page` and `entry` links) |
| `widget.entry_uuid` | string\|null | Entry UUID (for `entry` links) |
| `widget.collection_code` | string\|null | Collection code (for `entry` links) |
| `widget.entry_code` | string\|null | Entry code (for `entry` links) |
| `settings` | object | Style settings (optional) |

## Example Response (Custom URL)

```json
{
  "widget_type": "button",
  "uuid": "btn-123",
  "widget": {
    "link_type": "custom",
    "style": "primary",
    "size": "md",
    "target_blank": false,
    "border_radius": "md",
    "padding": null,
    "icon": null,
    "icon_position": "left",
    "text": {
      "en": "Learn More",
      "pl": "Dowiedz sie wiecej"
    },
    "url": "https://example.com/contact"
  },
  "settings": {
    "horizontalAlign": "center",
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Example Response (Page Link)

```json
{
  "widget_type": "button",
  "uuid": "btn-456",
  "widget": {
    "link_type": "page",
    "style": "secondary",
    "size": "lg",
    "target_blank": false,
    "icon": "fa-solid fa-envelope",
    "icon_position": "left",
    "text": {
      "en": "Contact Us",
      "pl": "Kontakt"
    },
    "page_uuid": "abc-123",
    "page_code": "contact",
    "route_uuid": "route-789",
    "url": "/contact"
  },
  "settings": {}
}
```

## Link Types

| Value | Description |
|-------|-------------|
| `custom` | Custom URL (use `widget.url`) |
| `page` | Link to a page (use `widget.url` or `widget.page_code`) |
| `entry` | Link to a collection entry (use `widget.url` or `widget.entry_code`) |

## Button Styles

| Value | Description |
|-------|-------------|
| `primary` | Primary action button |
| `secondary` | Secondary action button |
| `outline` | Outlined button |

## Multi-Item Support

The button widget supports displaying multiple buttons in a grid. When multiple items are configured, the API returns a multi-item structure with `multi_item: true`, `multi_columns`, and `items[]` array. Each item has the same structure as a single button widget (without `uuid` and `settings`).

```json
{
  "widget_type": "button",
  "uuid": "btn-multi",
  "multi_item": true,
  "multi_columns": 2,
  "multi_gap": 16,
  "items": [
    {
      "widget_type": "button",
      "widget": { "link_type": "custom", "style": "primary", "size": "md", "target_blank": false, "icon": null, "icon_position": "left", "text": { "en": "Learn More" }, "url": "/about" }
    },
    {
      "widget_type": "button",
      "widget": { "link_type": "custom", "style": "outline", "size": "md", "target_blank": false, "icon": null, "icon_position": "left", "text": { "en": "Contact" }, "url": "/contact" }
    }
  ],
  "settings": {}
}
```

## Usage Example

```javascript
function renderButton(widget, language) {
  const { style, size, target_blank, icon, icon_position } = widget.widget;
  const text = widget.widget?.text?.[language] || widget.widget?.text?.en || '';
  const url = widget.widget?.url || '#';
  const target = target_blank ? ' target="_blank" rel="noopener"' : '';
  const iconHtml = icon ? `<i class="${icon}"></i> ` : '';

  const content = icon_position === 'right'
    ? `${text} ${iconHtml}`
    : `${iconHtml}${text}`;

  return `<a href="${url}" class="btn btn-${style} btn-${size}"${target}>${content}</a>`;
}

function renderButtonWidget(widget, language) {
  if (widget.multi_item) {
    return `<div style="display: grid; grid-template-columns: repeat(${widget.multi_columns}, 1fr); gap: ${widget.multi_gap}px;">
      ${widget.items.map(item => renderButton(item, language)).join('')}
    </div>`;
  }
  return renderButton(widget, language);
}
```
