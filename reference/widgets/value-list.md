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
| `widget.group_field` | string\|null | Field code to group items by |
| `widget.display_style` | string | Display style: `"list"`, `"inline"`, `"tags"`, `"buttons"`, `"cards"` (default: `"list"`) |
| `widget.columns` | number | Number of columns for layout (default: 1) |
| `widget.items_gap` | number\|null | Gap between items in pixels |
| `widget.subtitle_field` | string\|null | Field code for card subtitle (used with `"cards"` style) |
| `widget.icon_field` | string\|null | Field code for card icon class (used with `"cards"` style) |
| `widget.show_count` | boolean | Show count of entries per value (default: `false`) |
| `widget.filter_field` | string\|null | Field code to filter entries by |
| `widget.filter_value` | string\|null | Static value to filter by |
| `widget.filter_value_source` | string | Filter value source: `"static"`, `"url"`, or `"entry_field"` (default: `"static"`) |
| `widget.filter_url_segment` | number | URL path segment number to extract filter value from (default: `1`) |
| `widget.filter_entry_field_code` | string\|null | Field code from current entry to use as filter value |
| `widget.sort_field` | string\|null | Field code to sort items by |
| `widget.sort_dir` | string | Sort direction: `"asc"` or `"desc"` (default: `"asc"`) |
| `widget.visible_limit` | number | Maximum visible items, 0 = unlimited (default: 0) |
| `widget.show_more_text` | string\|null | Text for "show more" button |
| `widget.show_more_url` | string\|null | URL for "show more" button |
| `widget.link_enabled` | boolean | Make values clickable (default: `false`) |
| `widget.link_to_entry` | boolean | Link directly to entry detail page (default: `false`) |
| `widget.link_url_pattern` | string\|null | URL pattern with `{value}` placeholder |
| `widget.tag_bg_color` | string\|null | Tag background color (hex or `var:*`) |
| `widget.tag_border_color` | string\|null | Tag border color (hex or `var:*`) |
| `widget.tag_text_color` | string\|null | Tag text color (hex or `var:*`) |
| `widget.tag_padding` | string\|null | Tag padding (CSS value) |
| `widget.tag_font_size` | string\|null | Tag font size (CSS value) |
| `widget.tag_border_radius` | string\|null | Tag border radius (CSS value) |
| `widget.more_bg_color` | string\|null | "Show more" button background color (hex or `var:*`) |
| `widget.more_text_color` | string\|null | "Show more" button text color (hex or `var:*`) |
| `widget.card_icon` | string\|null | Default icon class for card style |
| `widget.card_icon_bg_color` | string\|null | Card icon background color (hex or `var:*`) |
| `widget.card_icon_color` | string\|null | Card icon color (hex or `var:*`) |
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
    "group_field": null,
    "display_style": "tags",
    "columns": 1,
    "items_gap": null,
    "subtitle_field": null,
    "icon_field": null,
    "show_count": true,
    "filter_field": null,
    "filter_value": null,
    "filter_value_source": "static",
    "filter_url_segment": 1,
    "filter_entry_field_code": null,
    "sort_field": null,
    "sort_dir": "asc",
    "visible_limit": 0,
    "show_more_text": null,
    "show_more_url": null,
    "link_enabled": true,
    "link_to_entry": false,
    "link_url_pattern": "/blog/category/{value}",
    "tag_bg_color": null,
    "tag_border_color": null,
    "tag_text_color": null,
    "tag_padding": null,
    "tag_font_size": null,
    "tag_border_radius": null,
    "more_bg_color": null,
    "more_text_color": null,
    "card_icon": null,
    "card_icon_bg_color": null,
    "card_icon_color": null
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
