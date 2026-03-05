# Value List Widget

Display unique values from a collection field as a list, tags, or buttons.

## Widget Type

```
value-list
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"value-list"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget data |
| `widget.collection_code` | string\|null | Collection code |
| `widget.value_field` | string\|null | Field code to extract values from |
| `widget.group_field` | string\|null | Field code to group values by |
| `widget.subtitle_field` | string\|null | Field code for card subtitle (used with `"cards"` style) |
| `widget.icon_field` | string\|null | Field code for card icon class (used with `"cards"` style) |
| `widget.display_style` | string | Display style: `"list"`, `"inline"`, `"tags"`, `"buttons"`, `"cards"` (default: `"list"`) |
| `widget.columns` | number | Number of columns for layout (default: `1`) |
| `widget.show_count` | boolean | Show count of entries per value (default: `false`) |
| `widget.filter_field` | string\|null | Field code to filter entries by |
| `widget.filter_value` | string\|null | Value to match for filtering |
| `widget.sort_field` | string\|null | Field code to sort values by |
| `widget.sort_dir` | string | Sort direction: `"asc"` or `"desc"` (default: `"asc"`) |
| `widget.visible_limit` | number | Maximum number of values to display initially (default: `0` = unlimited) |
| `widget.show_more_text` | string\|null | Text for the "show more" link |
| `widget.show_more_url` | string\|null | URL for the "show more" link |
| `widget.tag_bg_color` | string\|null | Background color for tag items (hex) |
| `widget.tag_border_color` | string\|null | Border color for tag items (hex) |
| `widget.tag_text_color` | string\|null | Text color for tag items (hex) |
| `widget.more_bg_color` | string\|null | Background color for "show more" button (hex) |
| `widget.more_text_color` | string\|null | Text color for "show more" button (hex) |
| `widget.tag_padding` | string\|null | Custom padding for tag items (e.g. `"6px 16px"`) |
| `widget.tag_font_size` | string\|null | Custom font size for tag items (e.g. `"14px"`) |
| `widget.tag_border_radius` | string\|null | Custom border radius for tag items (e.g. `"1rem"`) |
| `widget.items_gap` | string\|null | Gap between items (e.g. `"8px"`, `"12px 16px"`) |
| `widget.link_enabled` | boolean | Make values clickable (default: `false`) |
| `widget.link_url_pattern` | string\|null | URL pattern with `{value}` placeholder |
| `widget.card_icon_bg_color` | string\|null | Background color for card icon (used with `"cards"` style) |
| `widget.card_icon_color` | string\|null | Icon color for card icon (used with `"cards"` style) |
| `settings` | object | Style settings (optional) |

### Server-Side Enrichment Fields

When the API can resolve the collection data server-side, the following field is conditionally included in the response. When absent, fetch entries client-side via the Collection API.

| Property | Type | Description |
|----------|------|-------------|
| `widget.entries` | array | Pre-fetched collection entries for value extraction |

## Example Response

```json
{
  "widget_type": "value-list",
  "uuid": "valuelist-123",
  "widget": {
    "collection_code": "blog",
    "value_field": "category",
    "group_field": null,
    "display_style": "tags",
    "columns": 1,
    "show_count": true,
    "filter_field": null,
    "filter_value": null,
    "sort_field": null,
    "sort_dir": "asc",
    "visible_limit": 0,
    "show_more_text": null,
    "show_more_url": null,
    "tag_bg_color": "#f0f0f0",
    "tag_border_color": null,
    "tag_text_color": "#333333",
    "more_bg_color": null,
    "more_text_color": null,
    "tag_padding": "6px 16px",
    "tag_font_size": "14px",
    "tag_border_radius": "1rem",
    "items_gap": "8px",
    "link_enabled": true,
    "link_url_pattern": "/blog/category/{value}"
  },
  "settings": {
    "paddingTop": 20,
    "paddingBottom": 20
  }
}
```

## Example Response (Simple List)

```json
{
  "widget_type": "value-list",
  "uuid": "valuelist-456",
  "widget": {
    "collection_code": "products",
    "value_field": "brand",
    "group_field": null,
    "display_style": "list",
    "columns": 1,
    "show_count": false,
    "filter_field": null,
    "filter_value": null,
    "sort_field": null,
    "sort_dir": "asc",
    "visible_limit": 0,
    "show_more_text": null,
    "show_more_url": null,
    "tag_bg_color": null,
    "tag_border_color": null,
    "tag_text_color": null,
    "more_bg_color": null,
    "more_text_color": null,
    "tag_padding": null,
    "tag_font_size": null,
    "tag_border_radius": null,
    "items_gap": null,
    "link_enabled": false,
    "link_url_pattern": null
  },
  "settings": {}
}
```

## Display Style Values

| Value | Description |
|-------|-------------|
| `list` | Vertical list |
| `inline` | Comma-separated inline list |
| `tags` | Tag/chip style |
| `buttons` | Button style |
| `cards` | Card style with icon, title, subtitle and arrow |

## Usage Example

```javascript
// Render value list widget
async function renderValueList(widget, language, api) {
  const {
    collection_code, value_field, display_style,
    show_count, link_enabled, link_url_pattern
  } = widget.widget;

  // Entries may be pre-fetched by the API (server-side enrichment).
  // If widget.entries exists, use it directly. Otherwise, fetch client-side.
  const entries = widget.widget.entries || await api.getCollectionEntries(collection_code);

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
