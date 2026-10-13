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
| `widget.text.html` | object | Multilingual button label |
| `widget.config` | object | Config element group |
| `widget.config.style` | string | Button style preset: `"primary"`, `"secondary"`, `"outline"`, etc. |
| `widget.config.size` | string | Button size: `"sm"`, `"md"`, `"lg"` |
| `widget.config.border_radius` | string | Border radius preset: `"none"`, `"sm"`, `"md"`, `"lg"`, `"pill"`, `"circle"` (round button, diameter from `circle_size`) |
| `widget.config.padding` | string\|null | Custom padding CSS value |
| `widget.config.icon` | string\|null | Icon class (e.g. `"fa-solid fa-arrow-right"`) or SVG string prefixed with `"svg:"` |
| `widget.config.icon_position` | string | Icon position: `"left"` or `"right"` |
| `widget.config.icon_hover_only` | boolean | When `true` the icon is hidden and slides in on hover (Divi-style reveal) |
| `widget.config.hover_effect` | string | `"none"` or `"fill-up"` (fill slides up from the bottom on hover) |
| `widget.config.fill_color` | string\|null | Fill color for `fill-up` (`var:code` or hex); `null` = white |
| `widget.config.fill_text_color` | string\|null | Text color on hover for `fill-up`; `null` = dark |
| `widget.config.magnetic` | boolean | When `true` the button follows the cursor inside itself (pointer devices only, off with reduced motion) |
| `widget.config.magnetic_strength` | number | 0.1–0.6, share of the cursor offset applied as translate (default 0.3) |
| `widget.config.circle_size` | number\|null | Diameter in px for `border_radius: "circle"` (default 210) |
| `widget.config.circle_size_mobile` | number\|null | Diameter in px on mobile; falls back to `min(circle_size, 150px)` |
| `widget.link` | object | Link element group |
| `widget.link.url` | string | Resolved URL (or custom URL) |
| `widget.link.link_type` | string | `"custom"`, `"page"`, or `"entry"` |
| `widget.link.page_id` | string\|null | Page UUID (for `page` links) |
| `widget.link.page_code` | string\|null | Resolved page code (for `page` links, enriched by server) |
| `widget.link.collection_code` | string\|null | Collection code (for `entry` links) |
| `widget.link.entry_id` | string\|null | Entry UUID (for `entry` links) |
| `widget.link.entry_code` | string\|null | Resolved entry code (for `entry` links, enriched by server) |
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
      "html": {
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
      "icon_position": "left",
      "icon_hover_only": false,
      "hover_effect": "none",
      "fill_color": null,
      "fill_text_color": null,
      "magnetic": false,
      "magnetic_strength": 0.3,
      "circle_size": 210,
      "circle_size_mobile": null
    },
    "link": {
      "url": "/about-us",
      "link_type": "page",
      "page_id": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
      "page_code": "about-us",
      "collection_code": null,
      "entry_id": null,
      "entry_code": null,
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
| `page` | Link to a page (server-resolved URL in `widget.link.url`, `page_code` enriched) |
| `entry` | Link to a collection entry (server-resolved URL in `widget.link.url`, `entry_code` enriched) |

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
  const label = text.html?.[language] || text.html?.en || '';
  const url = link.url || '#';
  const target = link.target_blank ? ' target="_blank" rel="noopener"' : '';
  const iconHtml = config.icon ? `<i class="${config.icon}${config.icon_hover_only ? ' icon-hover-only' : ''}"></i> ` : '';

  const content = config.icon_position === 'right'
    ? `${label} ${iconHtml}`
    : `${iconHtml}${label}`;

  return `<a href="${url}" class="btn btn-${config.style} btn-${config.size}"${target}>${content}</a>`;
}
```
