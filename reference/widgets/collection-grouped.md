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
| `config.collection_code` | string\|null | Collection code |
| `config.route_uuid` | string\|null | Route UUID for link generation |
| `config.group_by_field` | string\|null | Field code to group entries by |
| `config.style` | string | Display style: `"sections"`, `"accordion"`, `"tabs"` (default: `"sections"`) |
| `config.item_layout` | string | Item layout: `"list"`, `"grid"` (default: `"list"`) |
| `config.posts_count` | number | Maximum entries per group (default: 50) |
| `config.title_field` | string\|null | Field code for entry title |
| `config.description_field` | string\|null | Field code for description |
| `config.price_field` | string\|null | Field code for price |
| `config.image_field` | string\|null | Field code for image |
| `config.show_title` | boolean | Display entry title (default: true) |
| `config.show_description` | boolean | Display description (default: true) |
| `config.show_price` | boolean | Display price (default: true) |
| `config.show_image` | boolean | Display image (default: false) |
| `config.show_uncategorized` | boolean | Show entries without group value (default: true) |
| `config.use_custom_layout` | boolean | Use custom layout (legacy, default: false) |
| `config.layout_config` | object\|null | Custom layout configuration (legacy) |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "collection-grouped",
  "uuid": "grouped-123",
  "config": {
    "collection_code": "menu_items",
    "route_uuid": null,
    "group_by_field": "category",
    "style": "sections",
    "item_layout": "list",
    "posts_count": 50,
    "title_field": "name",
    "description_field": "description",
    "price_field": "price",
    "image_field": "photo",
    "show_title": true,
    "show_description": true,
    "show_price": true,
    "show_image": false,
    "show_uncategorized": true,
    "use_custom_layout": false,
    "layout_config": null
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

## Item Layout Values

| Value | Description |
|-------|-------------|
| `list` | Vertical list layout |
| `grid` | Grid layout |

## Usage Example

```javascript
function renderCollectionGrouped(widget, language) {
  const {
    style, item_layout,
    title_field, description_field, price_field, image_field,
    show_title, show_description, show_price, show_image, show_uncategorized
  } = widget.config;

  const groups = widget.data?.groups || {};
  const ungrouped = widget.data?.ungrouped || [];

  let html = '';

  Object.entries(groups).forEach(([groupName, entries]) => {
    html += `
      <section class="grouped-section">
        <h2 class="section-title">${groupName}</h2>
        <div class="section-items layout-${item_layout}">
          ${renderItems(entries, { title_field, description_field, price_field, image_field, show_title, show_description, show_price, show_image }, language)}
        </div>
      </section>
    `;
  });

  if (show_uncategorized && ungrouped.length > 0) {
    html += `
      <section class="grouped-section">
        <h2 class="section-title">Other</h2>
        <div class="section-items layout-${item_layout}">
          ${renderItems(ungrouped, { title_field, description_field, price_field, image_field, show_title, show_description, show_price, show_image }, language)}
        </div>
      </section>
    `;
  }

  return `<div class="collection-grouped style-${style}">${html}</div>`;
}

function renderItems(entries, config, language) {
  return entries.map(entry => {
    const title = config.title_field ? (entry.data[config.title_field]?.[language] || '') : '';
    const description = config.description_field ? (entry.data[config.description_field]?.[language] || '') : '';
    const price = config.price_field ? entry.data[config.price_field] : '';
    const image = config.image_field ? entry.data[config.image_field] : '';

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
