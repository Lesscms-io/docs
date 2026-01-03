# Value List Widget

Display unique values from a collection field as a list, tags, or buttons.

## Widget Type

```
value-list
```

## Data Properties

| Property | Type | Global | Description |
|----------|------|--------|-------------|
| `collection_code` | string | Yes | Collection code |
| `value_field` | string | Yes | Field code to extract values from |
| `display_style` | string | Yes | Display style: `list`, `inline`, `tags`, `buttons` |
| `show_count` | boolean | Yes | Show count of entries per value |
| `link_enabled` | boolean | Yes | Make values clickable |
| `link_url_pattern` | string | Yes | URL pattern with `{value}` placeholder |

## Example Response

```json
{
  "widget_type": "value-list",
  "uuid": "valuelist-123",
  "data": {
    "collection_code": "blog",
    "value_field": "category",
    "display_style": "tags",
    "show_count": true,
    "link_enabled": true,
    "link_url_pattern": "/blog/category/{value}"
  },
  "settings": {
    "paddingTop": 20,
    "paddingBottom": 20
  }
}
```

## Display Style Values

| Value | Description |
|-------|-------------|
| `list` | Vertical list |
| `inline` | Comma-separated inline list |
| `tags` | Tag/chip style |
| `buttons` | Button style |

## Usage Example

```javascript
// Render value list widget
async function renderValueList(widget, language, api) {
  const {
    collection_code, value_field, display_style,
    show_count, link_enabled, link_url_pattern
  } = widget.data;

  // Fetch all entries and extract unique values
  const entries = await api.getCollectionEntries(collection_code);

  // Count values
  const valueCounts = {};
  entries.forEach(entry => {
    const value = entry.data[value_field];
    // Handle multilingual fields
    const valueText = typeof value === 'object' ? value[language] || value.en : value;

    if (valueText) {
      // Handle arrays (multiselect fields)
      const values = Array.isArray(valueText) ? valueText : [valueText];
      values.forEach(v => {
        const label = typeof v === 'object' ? v.value || v.label : v;
        valueCounts[label] = (valueCounts[label] || 0) + 1;
      });
    }
  });

  const uniqueValues = Object.keys(valueCounts);

  if (uniqueValues.length === 0) {
    return '';
  }

  const items = uniqueValues.map(value => {
    const count = valueCounts[value];
    const url = link_enabled
      ? link_url_pattern.replace('{value}', encodeURIComponent(value))
      : null;

    const content = `${value}${show_count ? ` (${count})` : ''}`;

    if (url) {
      return `<a href="${url}" class="value-item">${content}</a>`;
    }
    return `<span class="value-item">${content}</span>`;
  }).join(display_style === 'inline' ? ', ' : '');

  return `
    <div class="value-list style-${display_style}">
      ${items}
    </div>
  `;
}
```

## CSS Example

```css
/* List Style */
.style-list {
  list-style: none;
  padding: 0;
}

.style-list .value-item {
  display: block;
  padding: 8px 0;
  border-bottom: 1px solid #eee;
  color: #333;
  text-decoration: none;
}

.style-list a.value-item:hover {
  color: #007BFF;
}

/* Inline Style */
.style-inline {
  line-height: 1.8;
}

.style-inline a {
  color: #007BFF;
}

/* Tags Style */
.style-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}

.style-tags .value-item {
  display: inline-block;
  padding: 6px 12px;
  background: #F0F0F0;
  border-radius: 16px;
  font-size: 14px;
  color: #333;
  text-decoration: none;
}

.style-tags a.value-item:hover {
  background: #007BFF;
  color: white;
}

/* Buttons Style */
.style-buttons {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}

.style-buttons .value-item {
  display: inline-block;
  padding: 10px 20px;
  background: #007BFF;
  color: white;
  border-radius: 4px;
  font-size: 14px;
  font-weight: 500;
  text-decoration: none;
}

.style-buttons a.value-item:hover {
  background: #0056b3;
}
```

## Use Cases

- **Category filters**: List blog categories with post counts
- **Tag clouds**: Display all tags used in a collection
- **Filter buttons**: Create filter UI for collection items
- **Sidebar widgets**: Show popular categories/tags
