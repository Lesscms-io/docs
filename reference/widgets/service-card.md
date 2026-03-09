# Service Card Widget

A business card widget for displaying services with badge, icon, title, description and call-to-action link. Perfect for showcasing service offerings, features, or pricing tiers.

## Widget Type

```
service-card
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"service-card"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget data |
| `widget.icon` | string | Icon class (e.g., `"fa-solid fa-tree"`) |
| `widget.icon_size` | number | Icon container size in pixels (default: 48) |
| `widget.icon_color` | string | Icon color (hex) |
| `widget.icon_background` | string | Icon background color |
| `widget.highlighted` | boolean | Whether the card is highlighted/featured (default: false) |
| `widget.link_url` | string | URL for the call-to-action link |
| `widget.link_type` | string | Link type: `"custom"`, `"page"`, `"entry"`, `"route"` (default: `"custom"`) |
| `widget.link_page_id` | string\|null | Page ID (when link_type is `"page"`) |
| `widget.link_collection_code` | string\|null | Collection code (when link_type is `"entry"`) |
| `widget.link_entry_id` | string\|null | Entry ID (when link_type is `"entry"`) |
| `widget.link_route_uuid` | string\|null | Route UUID (when link_type is `"route"`) |
| `widget.link_target_blank` | boolean | Open link in new tab (default: false) |
| `widget.link_color` | string\|null | Link text color |
| `widget.show_link` | boolean | Whether to show the link (default: true) |
| `widget.badge_color` | string | Badge text color |
| `widget.badge_background` | string | Badge background color |
| `widget.text_color` | string | Card text color |
| `widget.background_color` | string | Card background color |
| `widget.show_badge` | boolean | Whether to show the badge |
| `widget.border_radius` | number | Card border radius in pixels (default: 16) |
| `widget.hover_background_color` | string\|null | Background color on hover |
| `widget.hover_text_color` | string\|null | Text color on hover |
| `widget.hover_icon_color` | string\|null | Icon color on hover |
| `widget.hover_icon_background` | string\|null | Icon background color on hover |
| `widget.hover_lift` | number | Hover lift in pixels — translateY offset (default: `0`) |
| `widget.hover_scale` | number | Hover scale factor (default: `1`) |
| `widget.hover_shadow` | string | Hover shadow preset: `"none"`, `"sm"`, `"md"`, `"lg"` (default: `"none"`) |
| `widget.transition_duration` | number | Hover transition duration in ms (default: 200) |
| `widget.badge` | object | Multilingual badge text (optional) |
| `widget.title` | object | Multilingual title text |
| `widget.description` | object | Multilingual description text |
| `widget.link_text` | object | Multilingual link text |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "service-card",
  "uuid": "service-123",
  "widget": {
    "icon": "fa-solid fa-tree",
    "icon_size": 48,
    "icon_color": "#2e7d32",
    "icon_background": "#e8f5e9",
    "highlighted": false,
    "link_url": "/services/tree-cutting",
    "link_type": "custom",
    "link_page_id": null,
    "link_collection_code": null,
    "link_entry_id": null,
    "link_route_uuid": null,
    "link_target_blank": false,
    "link_color": "var:primary",
    "show_link": true,
    "badge_color": null,
    "badge_background": null,
    "text_color": "var:dark",
    "background_color": "var:light",
    "show_badge": false,
    "border_radius": 16,
    "hover_background_color": null,
    "hover_text_color": null,
    "hover_icon_color": null,
    "hover_icon_background": null,
    "hover_lift": 4,
    "hover_scale": 1,
    "hover_shadow": "sm",
    "transition_duration": 200,
    "badge": {},
    "title": {
      "en": "Tree Cutting",
      "pl": "Wycinka drzew"
    },
    "description": {
      "en": "Safe tree removal using climbing or platform techniques.",
      "pl": "Bezpieczne usuwanie drzew metoda alpinistyczna lub z wykorzystaniem podnosnika."
    },
    "link_text": {
      "en": "Learn more",
      "pl": "Dowiedz sie wiecej"
    }
  },
  "settings": {}
}
```

## Example Response (With Badge)

```json
{
  "widget_type": "service-card",
  "uuid": "service-456",
  "widget": {
    "icon": "fa-solid fa-cut",
    "icon_size": 48,
    "icon_color": "#ffffff",
    "icon_background": "rgba(255, 255, 255, 0.15)",
    "highlighted": true,
    "link_url": "/services/tree-pruning",
    "link_type": "custom",
    "link_page_id": null,
    "link_collection_code": null,
    "link_entry_id": null,
    "link_route_uuid": null,
    "link_target_blank": false,
    "badge_color": "#1a4d3e",
    "badge_background": "#4ade80",
    "text_color": "#ffffff",
    "background_color": "#1a4d3e",
    "show_badge": true,
    "border_radius": 16,
    "hover_background_color": null,
    "hover_text_color": null,
    "hover_lift": 0,
    "hover_scale": 1,
    "hover_shadow": "none",
    "transition_duration": 200,
    "badge": {
      "en": "MOST POPULAR",
      "pl": "NAJPOPULARNIEJSZA"
    },
    "title": {
      "en": "Tree Pruning",
      "pl": "Przycinanie drzew"
    },
    "description": {
      "en": "Professional crown care. We perform formative, clearing and sanitary cuts.",
      "pl": "Profesjonalna pielegnacja koron drzew. Wykonujemy ciecia formujace, przeswietlajace i sanitarne."
    },
    "link_text": {
      "en": "Learn more",
      "pl": "Dowiedz sie wiecej"
    }
  },
  "settings": {}
}
```

## Multi-Item Support

The service-card widget supports displaying multiple cards in a grid. When multiple items are configured, the API returns a multi-item structure:

```json
{
  "widget_type": "service-card",
  "uuid": "service-multi",
  "multi_item": true,
  "multi_columns": 3,
  "multi_gap": 16,
  "items": [
    {
      "widget_type": "service-card",
      "widget": { "icon": "fa-solid fa-tree", "icon_color": "#2e7d32", "icon_background": "#e8f5e9", "link_url": "/services/tree-cutting", "show_badge": false, "badge": {}, "title": { "en": "Tree Cutting" }, "description": { "en": "Safe tree removal." }, "link_text": { "en": "Learn more" } }
    },
    {
      "widget_type": "service-card",
      "widget": { "icon": "fa-solid fa-cut", "icon_color": "#2e7d32", "icon_background": "#e8f5e9", "link_url": "/services/pruning", "show_badge": false, "badge": {}, "title": { "en": "Tree Pruning" }, "description": { "en": "Professional crown care." }, "link_text": { "en": "Learn more" } }
    },
    {
      "widget_type": "service-card",
      "widget": { "icon": "fa-solid fa-leaf", "icon_color": "#2e7d32", "icon_background": "#e8f5e9", "link_url": "/services/planting", "show_badge": false, "badge": {}, "title": { "en": "Tree Planting" }, "description": { "en": "Expert planting service." }, "link_text": { "en": "Learn more" } }
    }
  ],
  "settings": {}
}
```

Per-item fields (`badge`, `icon`, `icon_size`, `title`, `description`, `link_text`, `link_url`, `link_color`, `show_link`, `icon_color`, `icon_background`, `badge_color`, `badge_background`, `text_color`, `background_color`, `show_badge`, `border_radius`, `hover_background_color`, `hover_text_color`, `hover_icon_color`, `hover_icon_background`, `transition_duration`) are unique to each item.

## Usage Example

```javascript
function renderServiceCard(widget, language) {
  const { icon, icon_color, icon_background, link_url, badge_color, badge_background, text_color, background_color, show_badge, border_radius, hover_background_color, hover_text_color, transition_duration } = widget.widget;
  const { badge, title, description, link_text } = widget.widget;

  const badgeText = badge?.[language] || badge?.en || '';
  const titleText = title?.[language] || title?.en || '';
  const descText = description?.[language] || description?.en || '';
  const linkTextValue = link_text?.[language] || link_text?.en || '';

  const dur = transition_duration || 200;
  const cardStyle = [
    text_color ? `color: ${text_color}` : '',
    background_color ? `background: ${background_color}` : '',
    border_radius != null ? `border-radius: ${border_radius}px` : '',
    `transition: background-color ${dur}ms ease, color ${dur}ms ease`
  ].filter(Boolean).join('; ');

  let badgeHtml = '';
  if (show_badge && badgeText) {
    const badgeStyle = `color: ${badge_color || '#1a4d3e'}; background: ${badge_background || '#4ade80'};`;
    badgeHtml = `<span class="service-card__badge" style="${badgeStyle}">${badgeText}</span>`;
  }

  const iconStyle = `color: ${icon_color || '#2e7d32'}; background: ${icon_background || '#e8f5e9'};`;

  return `
    <div class="service-card" style="${cardStyle}">
      ${badgeHtml}
      <div class="service-card__icon" style="${iconStyle}">
        <i class="${icon}"></i>
      </div>
      <h3 class="service-card__title">${titleText}</h3>
      <p class="service-card__description">${descText}</p>
      ${linkTextValue && link_url ? `<a href="${link_url}" class="service-card__link">${linkTextValue} &rarr;</a>` : ''}
    </div>
  `;
}
```

## CSS Example

```css
.service-card {
  display: flex;
  flex-direction: column;
  padding: 2rem;
  background: #fff;
  /* border_radius from API, default 16px */
  border-radius: 16px;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
  position: relative;
  /* transition_duration from API, default 200ms */
  transition: background-color 200ms ease, color 200ms ease;
}

/* Apply hover_background_color and hover_text_color when set */
.service-card:hover {
  /* Use values from hover_background_color / hover_text_color */
}

.service-card__badge {
  position: absolute;
  top: 1rem;
  right: 1rem;
  padding: 0.375rem 0.75rem;
  font-size: 0.75rem;
  font-weight: 600;
  text-transform: uppercase;
  border-radius: 9999px;
}

.service-card__icon {
  width: 3.5rem;
  height: 3.5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 0.75rem;
  font-size: 1.5rem;
  margin-bottom: 1.25rem;
}

.service-card__title {
  font-size: 1.25rem;
  font-weight: 700;
  margin: 0 0 0.75rem 0;
}

.service-card__description {
  font-size: 0.9375rem;
  line-height: 1.6;
  margin: 0 0 1.5rem 0;
  flex: 1;
}

.service-card__link {
  font-size: 0.9375rem;
  font-weight: 500;
  text-decoration: none;
}
```
