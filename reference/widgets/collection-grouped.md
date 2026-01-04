# Collection Grouped Widget

Display collection entries grouped by a field value (e.g., categories, sections).

## Widget Type

```
collection-grouped
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"collection-grouped"` |
| `uuid` | string | Unique widget identifier |
| `config` | object | Widget configuration |
| `config.collection_code` | string | Collection code |
| `config.group_by_field` | string | Field code to group entries by |
| `config.display_style` | string | Display style: `"sections"`, `"accordion"`, `"tabs"` |
| `config.posts_count` | number | Maximum entries per group |
| `config.title_field` | string | Field code for entry title |
| `config.description_field` | string | Field code for description |
| `config.price_field` | string | Field code for price |
| `config.image_field` | string | Field code for image |
| `config.link_field` | string | Field for entry URL |
| `config.show_title` | boolean | Display entry title |
| `config.show_description` | boolean | Display description |
| `config.show_price` | boolean | Display price |
| `config.show_image` | boolean | Display image |
| `config.show_uncategorized` | boolean | Show entries without group value |
| `data` | object | Fetched and grouped entries |
| `data.groups` | object | Object with group keys and entry arrays |
| `data.ungrouped` | array | Entries without group value |
| `settings` | object | Style settings (optional) |

## Example Response (Restaurant Menu)

```json
{
  "widget_type": "collection-grouped",
  "uuid": "grouped-123",
  "config": {
    "collection_code": "menu_items",
    "group_by_field": "category",
    "display_style": "sections",
    "posts_count": 50,
    "title_field": "name",
    "description_field": "description",
    "price_field": "price",
    "image_field": "photo",
    "link_field": "url",
    "show_title": true,
    "show_description": true,
    "show_price": true,
    "show_image": false,
    "show_uncategorized": false
  },
  "data": {
    "groups": {
      "Appetizers": [
        {
          "uuid": "item-1",
          "url": "/menu/spring-rolls",
          "data": {
            "name": { "en": "Spring Rolls", "pl": "Sajgonki" },
            "description": { "en": "Crispy vegetable rolls", "pl": "Chrupiące roladki warzywne" },
            "price": "$8.99",
            "category": [{ "code": "appetizers", "value": "Appetizers" }]
          }
        }
      ],
      "Main Courses": [
        {
          "uuid": "item-2",
          "url": "/menu/pad-thai",
          "data": {
            "name": { "en": "Pad Thai", "pl": "Pad Thai" },
            "description": { "en": "Stir-fried noodles", "pl": "Smażony makaron" },
            "price": "$14.99",
            "category": [{ "code": "main", "value": "Main Courses" }]
          }
        }
      ]
    },
    "ungrouped": []
  },
  "settings": {
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Display Style Values

| Value | Description |
|-------|-------------|
| `sections` | Groups as separate sections with headers |
| `accordion` | Collapsible accordion panels |
| `tabs` | Tabbed interface |

## Usage Example

```javascript
function renderCollectionGrouped(widget, language) {
  const {
    display_style, title_field, description_field, price_field, image_field,
    show_title, show_description, show_price, show_image
  } = widget.config;

  const groups = widget.data?.groups || {};
  const ungrouped = widget.data?.ungrouped || [];

  let html = '';

  Object.entries(groups).forEach(([groupName, entries]) => {
    html += `
      <section class="grouped-section">
        <h2 class="section-title">${groupName}</h2>
        <div class="section-items">
          ${renderItems(entries, { title_field, description_field, price_field, image_field, show_title, show_description, show_price, show_image }, language)}
        </div>
      </section>
    `;
  });

  if (ungrouped.length > 0) {
    html += `
      <section class="grouped-section">
        <h2 class="section-title">Other</h2>
        <div class="section-items">
          ${renderItems(ungrouped, { title_field, description_field, price_field, image_field, show_title, show_description, show_price, show_image }, language)}
        </div>
      </section>
    `;
  }

  return `<div class="collection-grouped style-${display_style}">${html}</div>`;
}

function renderItems(entries, config, language) {
  return entries.map(entry => {
    const title = entry.data[config.title_field]?.[language] || '';
    const description = entry.data[config.description_field]?.[language] || '';
    const price = entry.data[config.price_field] || '';
    const image = entry.data[config.image_field] || '';

    return `
      <div class="grouped-item">
        ${config.show_image && image ? `<img src="${image}" alt="${title}">` : ''}
        <div class="item-info">
          ${config.show_title ? `<h3 class="item-title">${title}</h3>` : ''}
          ${config.show_description ? `<p class="item-description">${description}</p>` : ''}
        </div>
        ${config.show_price ? `<span class="item-price">${price}</span>` : ''}
      </div>
    `;
  }).join('');
}
```

## Use Cases

- **Restaurant menus**: Group dishes by category
- **Product catalogs**: Group products by category
- **FAQ sections**: Group questions by topic
- **Team pages**: Group members by department
- **Event schedules**: Group sessions by day/track
