# Value List Widget

Display unique values from a collection field as a list, tags, buttons, or cards.

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
| `widget.collection_code` | string\|null | Collection code to extract values from |
| `widget.value_field` | string\|null | Field code to extract unique values from |
| `widget.display_style` | string | Display style: `"list"`, `"inline"`, `"tags"`, `"buttons"`, `"cards"` (default: `"list"`) |
| `widget.subtitle_field` | string\|null | Field code for card subtitle (used with `"cards"` style) |
| `widget.icon_field` | string\|null | Field code for card icon class (used with `"cards"` style) |
| `widget.show_count` | boolean | Show count of entries per value (default: `false`) |
| `widget.filter_value_source` | string | Filter value source: `"static"`, `"url"`, or `"entry_field"` (default: `"static"`) |
| `widget.filter_url_segment` | number | URL path segment number to extract filter value from (default: `1`) |
| `widget.filter_entry_field_code` | string\|null | Field code from current entry to use as filter value |
| `widget.link_enabled` | boolean | Make values clickable (default: `false`) |
| `widget.link_url_pattern` | string\|null | URL pattern with `{value}` placeholder |
| `settings` | object | Style settings (optional) |

### Server-Side Enrichment

When the API can resolve the collection data server-side, it includes `widget.entries` with pre-fetched collection entries. When absent, fetch entries client-side via the Collection API.

## Example Response

```json
{
  "widget_type": "value-list",
  "uuid": "valuelist-123",
  "widget": {
    "collection_code": "blog",
    "value_field": "category",
    "display_style": "tags",
    "subtitle_field": null,
    "icon_field": null,
    "show_count": true,
    "filter_value_source": "static",
    "filter_url_segment": 1,
    "filter_entry_field_code": null,
    "link_enabled": true,
    "link_url_pattern": "/blog/category/{value}"
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
| `list` | Vertical list |
| `inline` | Comma-separated inline list |
| `tags` | Tag/chip style |
| `buttons` | Button style |
| `cards` | Card style with icon, title, subtitle and arrow |

## Usage Example

```javascript
async function renderValueList(widget, language, api) {
  const { collection_code, value_field, display_style, show_count, link_enabled, link_url_pattern } = widget.widget;

  const entries = widget.widget.entries || await api.getCollectionEntries(collection_code);

  const valueCounts = {};
  entries.forEach(entry => {
    const value = entry.data[value_field];
    const values = Array.isArray(value) ? value : [value];
    values.forEach(v => {
      const label = typeof v === 'object' ? v.value || v.label : v;
      if (label) valueCounts[label] = (valueCounts[label] || 0) + 1;
    });
  });

  const items = Object.entries(valueCounts).map(([value, count]) => {
    const content = `${value}${show_count ? ` (${count})` : ''}`;
    const url = link_enabled ? link_url_pattern.replace('{value}', encodeURIComponent(value)) : null;

    if (url) return `<a href="${url}" class="value-item">${content}</a>`;
    return `<span class="value-item">${content}</span>`;
  }).join(display_style === 'inline' ? ', ' : '');

  return `<div class="value-list style-${display_style}">${items}</div>`;
}
```

## Use Cases

- **Category filters**: List blog categories with post counts
- **Tag clouds**: Display all tags used in a collection
- **Filter buttons**: Create filter UI for collection items
- **Sidebar widgets**: Show popular categories/tags
