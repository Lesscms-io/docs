# Service Card Widget

A card widget for displaying services with icon, title, description, link and optional badge. Uses element-group governance — each visual element is a nested object.

## Widget Type

```
service-card
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"service-card"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget data (element groups) |
| `widget.heading` | object | Heading element group |
| `widget.heading.content` | object | Multilingual heading text |
| `widget.heading.color` | string\|null | Heading text color |
| `widget.heading.color:hover` | string\|null | Heading text color on hover |
| `widget.description` | object | Description element group |
| `widget.description.content` | object | Multilingual description text |
| `widget.description.color` | string\|null | Description text color |
| `widget.description.color:hover` | string\|null | Description text color on hover |
| `widget.icon` | object | Icon element group |
| `widget.icon.icon` | string\|null | Icon class (e.g., `"fa-solid fa-gear"`) or SVG (`"svg:..."`) |
| `widget.icon.size` | number | Icon size in pixels (default: 48) |
| `widget.icon.padding` | number | Icon padding in pixels (default: 10) |
| `widget.icon.color` | string\|null | Icon color |
| `widget.icon.color:hover` | string\|null | Icon color on hover |
| `widget.icon.background` | string\|null | Icon background color |
| `widget.icon.background:hover` | string\|null | Icon background color on hover |
| `widget.link` | object | Link element group |
| `widget.link.content` | object | Multilingual link text |
| `widget.link.url` | string\|null | Resolved link URL (server-side resolved from page/entry/route) |
| `widget.link.show` | boolean | Whether to show the link (default: true) |
| `widget.link.color` | string\|null | Link text color |
| `widget.link.color:hover` | string\|null | Link text color on hover |
| `widget.link.target_blank` | boolean | Open link in new tab (default: false) |
| `widget.badge` | object | Badge element group |
| `widget.badge.content` | object | Multilingual badge text |
| `widget.badge.show` | boolean | Whether to show badge (default: false) |
| `widget.badge.color` | string\|null | Badge text color |
| `widget.badge.color:hover` | string\|null | Badge text color on hover |
| `widget.badge.background` | string\|null | Badge background color |
| `widget.badge.background:hover` | string\|null | Badge background color on hover |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "service-card",
  "uuid": "sc-001",
  "widget": {
    "heading": {
      "content": { "pl": "Wycinka drzew", "en": "Tree Cutting" },
      "color": "var:dark",
      "color:hover": "var:light"
    },
    "description": {
      "content": { "pl": "Bezpieczne usuwanie drzew.", "en": "Safe tree removal." },
      "color": "var:dark",
      "color:hover": "var:light"
    },
    "icon": {
      "icon": "fa-solid fa-tree",
      "size": 48,
      "padding": 10,
      "color": "var:white",
      "color:hover": "var:primary",
      "background": "var:primary",
      "background:hover": "var:white"
    },
    "link": {
      "content": { "pl": "Dowiedz si\u0119 wi\u0119cej", "en": "Learn more" },
      "url": "/services/tree-cutting",
      "show": true,
      "color": "var:primary",
      "color:hover": "var:light",
      "target_blank": false
    },
    "badge": {
      "content": {},
      "show": false,
      "color": "var:white",
      "color:hover": null,
      "background": "var:primary",
      "background:hover": null
    }
  },
  "settings": {
    "padding_top": 32,
    "padding_bottom": 32,
    "horizontal_align": "stretch",
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Wrapper Support

Service cards support wrapping — multiple cards can be grouped in a grid via the wrapper mechanism. The wrapper is a separate node in the page structure, not part of the widget itself.

## Usage Example

```javascript
function renderServiceCard(widget, language) {
  const { heading, description, icon, link, badge, style } = widget.widget;

  const title = heading.content?.[language] || heading.content?.pl || '';
  const desc = description.content?.[language] || description.content?.pl || '';
  const linkText = link.content?.[language] || link.content?.pl || '';
  const badgeText = badge.content?.[language] || badge.content?.pl || '';

  const dur = style.transition_duration || 200;

  return `
    <div class="service-card" style="
      background: ${resolveColor(style.background_color)};
      padding: ${style.padding_top}px ${style.padding_right}px ${style.padding_bottom}px ${style.padding_left}px;
      border-radius: ${style.border_radius}px;
      transition: all ${dur}ms ease;
    ">
      ${badge.show && badgeText ? `<span class="badge" style="color: ${resolveColor(badge.color)}; background: ${resolveColor(badge.background)}">${badgeText}</span>` : ''}
      <div class="icon" style="color: ${resolveColor(icon.color)}; background: ${resolveColor(icon.background)}; width: ${icon.size + icon.padding * 2}px; height: ${icon.size + icon.padding * 2}px; font-size: ${icon.size}px; padding: ${icon.padding}px">
        <i class="${icon.icon}"></i>
      </div>
      <h3 style="color: ${resolveColor(heading.color)}">${title}</h3>
      <p style="color: ${resolveColor(description.color)}">${desc}</p>
      ${link.show && linkText ? `<a href="${link.url}" style="color: ${resolveColor(link.color)}"${link.target_blank ? ' target="_blank" rel="noopener noreferrer"' : ''}>${linkText} &rarr;</a>` : ''}
    </div>
  `;
}
```
