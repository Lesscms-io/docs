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
| `config` | object | Widget configuration |
| `config.icon` | string | Icon class (e.g., `"fa-solid fa-tree"`) |
| `config.icon_color` | string | Icon color (hex) |
| `config.icon_background` | string | Icon background color |
| `config.highlighted` | boolean | Whether the card is highlighted/featured |
| `config.link_url` | string | URL for the call-to-action link |
| `config.badge_color` | string | Badge text color |
| `config.badge_background` | string | Badge background color |
| `content` | object | Multilingual content |
| `content.badge` | object | Multilingual badge text (optional) |
| `content.title` | object | Multilingual title text |
| `content.description` | object | Multilingual description text |
| `content.link_text` | object | Multilingual link text |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "service-card",
  "uuid": "service-123",
  "config": {
    "icon": "fa-solid fa-tree",
    "icon_color": "#2e7d32",
    "icon_background": "#e8f5e9",
    "highlighted": false,
    "link_url": "/services/tree-cutting",
    "badge_color": null,
    "badge_background": null
  },
  "content": {
    "badge": {},
    "title": {
      "en": "Tree Cutting",
      "pl": "Wycinka drzew"
    },
    "description": {
      "en": "Safe tree removal using climbing or platform techniques.",
      "pl": "Bezpieczne usuwanie drzew metodą alpinistyczną lub z wykorzystaniem podnośnika."
    },
    "link_text": {
      "en": "Learn more",
      "pl": "Dowiedz się więcej"
    }
  },
  "settings": {}
}
```

## Example Response (Highlighted with Badge)

```json
{
  "widget_type": "service-card",
  "uuid": "service-456",
  "config": {
    "icon": "fa-solid fa-cut",
    "icon_color": "#ffffff",
    "icon_background": "rgba(255, 255, 255, 0.15)",
    "highlighted": true,
    "link_url": "/services/tree-pruning",
    "badge_color": "#1a4d3e",
    "badge_background": "#4ade80"
  },
  "content": {
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
      "pl": "Profesjonalna pielęgnacja koron drzew. Wykonujemy cięcia formujące, prześwietlające i sanitarne."
    },
    "link_text": {
      "en": "Learn more",
      "pl": "Dowiedz się więcej"
    }
  },
  "settings": {}
}
```

## Usage Example

```javascript
function renderServiceCard(widget, language) {
  const { icon, icon_color, icon_background, highlighted, link_url, badge_color, badge_background } = widget.config;
  const { badge, title, description, link_text } = widget.content;

  const badgeText = badge?.[language] || badge?.en || '';
  const titleText = title?.[language] || title?.en || '';
  const descText = description?.[language] || description?.en || '';
  const linkTextValue = link_text?.[language] || link_text?.en || '';

  const cardClass = highlighted ? 'service-card service-card--highlighted' : 'service-card';

  let badgeHtml = '';
  if (badgeText) {
    const badgeStyle = `color: ${badge_color || '#1a4d3e'}; background: ${badge_background || '#4ade80'};`;
    badgeHtml = `<span class="service-card__badge" style="${badgeStyle}">${badgeText}</span>`;
  }

  const iconStyle = `color: ${icon_color || '#2e7d32'}; background: ${icon_background || '#e8f5e9'};`;

  return `
    <div class="${cardClass}">
      ${badgeHtml}
      <div class="service-card__icon" style="${iconStyle}">
        <i class="${icon}"></i>
      </div>
      <h3 class="service-card__title">${titleText}</h3>
      <p class="service-card__description">${descText}</p>
      ${linkTextValue && link_url ? `<a href="${link_url}" class="service-card__link">${linkTextValue} →</a>` : ''}
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
  border-radius: 1rem;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
  position: relative;
}

.service-card--highlighted {
  background: #1a4d3e;
  color: #fff;
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
