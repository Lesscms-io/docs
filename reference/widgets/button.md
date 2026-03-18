# Button Widget

A button widget with multilingual text, configurable style, and link target. Uses nested element-group structure.

## Widget Type

```
button
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"button"` |
| `uuid` | string | Unique widget identifier |
| `widget.text` | object | Text element group |
| `widget.text.content` | object | Multilingual button label |
| `widget.config` | object | Config element group |
| `widget.config.style` | string | Button style preset: `"primary"`, `"secondary"`, `"outline"`, etc. |
| `widget.config.size` | string | Button size: `"sm"`, `"md"`, `"lg"` |
| `widget.config.border_radius` | string | Border radius preset: `"none"`, `"sm"`, `"md"`, `"lg"`, `"pill"` |
| `widget.config.padding` | string\|null | Custom padding CSS value |
| `widget.config.icon` | string\|null | Icon class (e.g. `"fa-solid fa-arrow-right"`) or SVG string prefixed with `"svg:"` |
| `widget.config.icon_position` | string | Icon position: `"left"` or `"right"` |
| `widget.link` | object | Link element group |
| `widget.link.url` | string | Resolved URL (or custom URL) |
| `widget.link.link_type` | string | `"custom"`, `"page"`, or `"entry"` |
| `widget.link.page_id` | string\|null | Page UUID (for `page` links) |
| `widget.link.collection_code` | string\|null | Collection code (for `entry` links) |
| `widget.link.entry_id` | string\|null | Entry UUID (for `entry` links) |
| `widget.link.route_uuid` | string\|null | Route UUID for URL resolution |
| `widget.link.target_blank` | boolean | Open link in new tab |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "button",
  "uuid": "btn-123",
  "widget": {
    "text": {
      "content": {
        "en": "Learn More",
        "pl": "Dowiedz sie wiecej"
      }
    },
    "config": {
      "style": "primary",
      "size": "md",
      "border_radius": "md",
      "padding": null,
      "icon": null,
      "icon_position": "left"
    },
    "link": {
      "url": "https://example.com/contact",
      "link_type": "custom",
      "page_id": null,
      "collection_code": null,
      "entry_id": null,
      "route_uuid": null,
      "target_blank": false
    }
  },
  "settings": {
    "padding_top": 8,
    "padding_bottom": 8,
    "horizontal_align": "center",
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Link Types

| Value | Description |
|-------|-------------|
| `custom` | Custom URL (use `widget.link.url`) |
| `page` | Link to a page (server-resolved URL in `widget.link.url`) |
| `entry` | Link to a collection entry (server-resolved URL in `widget.link.url`) |

## Button Styles

| Value | Description |
|-------|-------------|
| `primary` | Primary action button |
| `secondary` | Secondary action button |
| `outline` | Outlined button |

## Usage Example

```javascript
function renderButton(widget, language) {
  const { text, config, link } = widget.widget;
  const label = text.content?.[language] || text.content?.en || '';
  const url = link.url || '#';
  const target = link.target_blank ? ' target="_blank" rel="noopener"' : '';
  const iconHtml = config.icon ? `<i class="${config.icon}"></i> ` : '';

  const content = config.icon_position === 'right'
    ? `${label} ${iconHtml}`
    : `${iconHtml}${label}`;

  return `<a href="${url}" class="btn btn-${config.style} btn-${config.size}"${target}>${content}</a>`;
}
```
