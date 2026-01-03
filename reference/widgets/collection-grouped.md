# Collection Grouped Widget

Display collection entries grouped by a field value (e.g., categories, sections).

## Widget Type

```
collection-grouped
```

## Data Properties

| Property | Type | Global | Description |
|----------|------|--------|-------------|
| `collection_code` | string | Yes | Collection code |
| `group_by_field` | string | Yes | Field code to group entries by |
| `style` | string | Yes | Display style: `sections`, `accordion`, `tabs` |
| `item_layout` | string | Yes | Item layout: `list`, `cards`, `compact` |
| `posts_count` | number | Yes | Maximum entries per group |
| `title_field` | string | Yes | Field code for entry title |
| `description_field` | string | Yes | Field code for description |
| `price_field` | string | Yes | Field code for price |
| `image_field` | string | Yes | Field code for image |
| `show_title` | boolean | Yes | Display entry title |
| `show_description` | boolean | Yes | Display description |
| `show_price` | boolean | Yes | Display price |
| `show_image` | boolean | Yes | Display image |
| `show_uncategorized` | boolean | Yes | Show entries without group value |

## Example Response (Restaurant Menu)

```json
{
  "widget_type": "collection-grouped",
  "uuid": "grouped-123",
  "data": {
    "collection_code": "menu_items",
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
    "show_uncategorized": false
  },
  "settings": {}
}
```

## Style Values

| Value | Description |
|-------|-------------|
| `sections` | Groups as separate sections with headers |
| `accordion` | Collapsible accordion panels |
| `tabs` | Tabbed interface |

## Item Layout Values

| Value | Description |
|-------|-------------|
| `list` | Simple list layout |
| `cards` | Card-based layout |
| `compact` | Minimal compact layout |

## Usage Example

```javascript
// Render collection grouped widget
async function renderCollectionGrouped(widget, language, api) {
  const {
    collection_code, group_by_field, style, item_layout,
    posts_count, title_field, description_field, price_field, image_field,
    show_title, show_description, show_price, show_image,
    show_uncategorized
  } = widget.data;

  // Fetch all entries
  const entries = await api.getCollectionEntries(collection_code, {
    limit: posts_count || 100
  });

  // Group entries by field value
  const groups = {};
  const ungrouped = [];

  entries.forEach(entry => {
    const groupValue = entry.data[group_by_field];
    let groupKey = '';

    // Handle multilingual or option fields
    if (typeof groupValue === 'object') {
      if (Array.isArray(groupValue) && groupValue.length > 0) {
        groupKey = groupValue[0]?.value || groupValue[0];
      } else {
        groupKey = groupValue[language] || groupValue.en || groupValue.value || '';
      }
    } else {
      groupKey = groupValue || '';
    }

    if (groupKey) {
      if (!groups[groupKey]) {
        groups[groupKey] = [];
      }
      groups[groupKey].push(entry);
    } else if (show_uncategorized) {
      ungrouped.push(entry);
    }
  });

  // Render based on style
  switch (style) {
    case 'accordion':
      return renderAccordion(groups, ungrouped, widget.data, language);
    case 'tabs':
      return renderTabs(groups, widget.data, language);
    default:
      return renderSections(groups, ungrouped, widget.data, language);
  }
}

function renderSections(groups, ungrouped, data, language) {
  let html = '';

  Object.entries(groups).forEach(([groupName, entries]) => {
    html += `
      <section class="grouped-section">
        <h2 class="section-title">${groupName}</h2>
        <div class="section-items layout-${data.item_layout}">
          ${renderItems(entries, data, language)}
        </div>
      </section>
    `;
  });

  if (ungrouped.length > 0) {
    html += `
      <section class="grouped-section">
        <h2 class="section-title">Other</h2>
        <div class="section-items layout-${data.item_layout}">
          ${renderItems(ungrouped, data, language)}
        </div>
      </section>
    `;
  }

  return `<div class="collection-grouped style-sections">${html}</div>`;
}

function renderItems(entries, data, language) {
  return entries.map(entry => {
    const title = entry.data[data.title_field]?.[language] || '';
    const description = entry.data[data.description_field]?.[language] || '';
    const price = entry.data[data.price_field] || '';
    const image = entry.data[data.image_field]?.url || '';

    return `
      <div class="grouped-item">
        ${data.show_image && image ? `<img src="${image}" alt="${title}">` : ''}
        <div class="item-info">
          ${data.show_title ? `<h3 class="item-title">${title}</h3>` : ''}
          ${data.show_description ? `<p class="item-description">${description}</p>` : ''}
        </div>
        ${data.show_price ? `<span class="item-price">${price}</span>` : ''}
      </div>
    `;
  }).join('');
}
```

## CSS Example (Restaurant Menu Style)

```css
.collection-grouped {
  max-width: 800px;
  margin: 0 auto;
}

.grouped-section {
  margin-bottom: 40px;
}

.section-title {
  font-size: 28px;
  font-weight: 600;
  padding-bottom: 12px;
  border-bottom: 2px solid #333;
  margin-bottom: 24px;
}

/* List Layout */
.layout-list .grouped-item {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  padding: 16px 0;
  border-bottom: 1px dotted #ccc;
}

.layout-list .item-info {
  flex: 1;
  padding-right: 20px;
}

.layout-list .item-title {
  font-size: 18px;
  font-weight: 500;
  margin: 0 0 4px;
}

.layout-list .item-description {
  font-size: 14px;
  color: #666;
  margin: 0;
}

.layout-list .item-price {
  font-size: 18px;
  font-weight: 600;
  white-space: nowrap;
}

/* Cards Layout */
.layout-cards {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 20px;
}

.layout-cards .grouped-item {
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  overflow: hidden;
}

.layout-cards .grouped-item img {
  width: 100%;
  height: 150px;
  object-fit: cover;
}

.layout-cards .item-info {
  padding: 16px;
}

/* Accordion Style */
.style-accordion .grouped-section {
  border: 1px solid #ddd;
  border-radius: 8px;
  margin-bottom: 8px;
}

.style-accordion .section-title {
  margin: 0;
  padding: 16px;
  border-bottom: none;
  cursor: pointer;
  display: flex;
  justify-content: space-between;
}

.style-accordion .section-items {
  padding: 0 16px 16px;
}
```

## Use Cases

- **Restaurant menus**: Group dishes by category
- **Product catalogs**: Group products by category
- **FAQ sections**: Group questions by topic
- **Team pages**: Group members by department
- **Event schedules**: Group sessions by day/track
