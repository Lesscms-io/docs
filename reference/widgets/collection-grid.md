# Collection Grid Widget

A grid display of collection entries with customizable layout and fields.

## Widget Type

```
collection-grid
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"collection-grid"` |
| `uuid` | string | Unique widget identifier |
| `config` | object | Widget configuration |
| `config.collection_code` | string | Collection code to display |
| `config.route_uuid` | string\|null | Route UUID for link generation |
| `config.columns` | number\|string | Number of columns (default: 3) |
| `config.posts_count` | number | Maximum entries to display (default: 6) |
| `config.card_style` | string | Card style: `"default"` |
| `config.entry_template` | string\|null | Entry template reference (e.g., `"custom:uuid"`) |
| `config.order_by` | string | Order field (default: `"created_at"`) |
| `config.order_dir` | string | Order direction: `"asc"`, `"desc"` (default: `"desc"`) |
| `config.title_field` | string\|null | Field code for title |
| `config.excerpt_field` | string\|null | Field code for excerpt |
| `config.image_field` | string\|null | Field code for image |
| `config.date_field` | string\|null | Field code for date |
| `config.extra_field` | string\|null | Field code for extra content |
| `config.show_title` | boolean | Display title (default: true) |
| `config.show_excerpt` | boolean | Display excerpt (default: true) |
| `config.show_image` | boolean | Display image (default: true) |
| `config.show_date` | boolean | Display date (default: true) |
| `config.show_read_more` | boolean | Display read more link (default: true) |
| `config.show_pagination` | boolean | Display pagination (default: false) |
| `config.show_extra` | boolean | Display extra field (default: false) |
| `config.read_more_text` | object | Multilingual read more text `{ "en": "...", "pl": "..." }` |
| `config.title_limit` | number | Character limit for title (0 = no limit) |
| `config.excerpt_limit` | number | Character limit for excerpt (0 = no limit) |
| `config.filter_field` | string\|null | Field code to filter by |
| `config.filter_source` | string | Filter source: `"static"`, `"url"` (default: `"static"`) |
| `config.filter_value` | string\|null | Static filter value |
| `config.filter_url_segment` | number\|null | URL segment index for dynamic filtering |
| `config.use_custom_layout` | boolean | Use custom layout (legacy, default: false) |
| `config.layout_config` | object\|null | Custom layout configuration (legacy) |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "collection-grid",
  "uuid": "81380123-3ad0-402b-afdd-8ef448174c79",
  "config": {
    "collection_code": "uslugi-marketingowe",
    "route_uuid": null,
    "columns": "4",
    "posts_count": 6,
    "card_style": "default",
    "entry_template": "custom:792b14fe-cd64-454c-933a-02e0718b3f3e",
    "order_by": "created_at",
    "order_dir": "desc",
    "title_field": null,
    "excerpt_field": null,
    "image_field": null,
    "date_field": null,
    "extra_field": null,
    "show_title": true,
    "show_excerpt": true,
    "show_image": true,
    "show_date": true,
    "show_read_more": true,
    "show_pagination": false,
    "show_extra": false,
    "read_more_text": {},
    "title_limit": 0,
    "excerpt_limit": 0,
    "filter_field": null,
    "filter_source": "static",
    "filter_value": null,
    "filter_url_segment": null,
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

## Entry Template

The `entry_template` field supports:

| Value | Description |
|-------|-------------|
| `null` | Use default card rendering with field mappings |
| `"custom:uuid"` | Use custom template defined in collection settings |

When using a custom template, the `title_field`, `excerpt_field`, `image_field`, `date_field` mappings are ignored - the template defines its own layout.

## Filtering

Entries can be filtered by a field value:

| Property | Description |
|----------|-------------|
| `filter_field` | The collection field code to filter by |
| `filter_source` | `"static"` = use `filter_value`, `"url"` = extract from URL |
| `filter_value` | The value to match when `filter_source` is `"static"` |
| `filter_url_segment` | URL segment index (1-based) when `filter_source` is `"url"` |

## Order Options

| order_by | Description |
|----------|-------------|
| `created_at` | Order by creation date |
| `updated_at` | Order by last update |
| `title` | Order alphabetically by title |
| `random` | Random order |

## Usage Example

```javascript
function renderCollectionGrid(widget, language) {
  const {
    columns,
    show_title, show_excerpt, show_image, show_date, show_read_more, show_extra,
    title_field, excerpt_field, image_field, date_field, extra_field,
    title_limit, excerpt_limit,
    read_more_text,
    entry_template
  } = widget.config;

  // If using custom template, render differently
  if (entry_template && entry_template.startsWith('custom:')) {
    return renderWithTemplate(widget, entry_template, language);
  }

  const entries = widget.data?.entries || [];
  const readMoreLabel = read_more_text?.[language] || 'Read More';

  if (entries.length === 0) return '<p>No entries found.</p>';

  const items = entries.map(entry => {
    let title = title_field ? (entry.data[title_field]?.[language] || '') : '';
    let excerpt = excerpt_field ? (entry.data[excerpt_field]?.[language] || '') : '';
    const image = image_field ? entry.data[image_field] : '';
    const date = date_field ? entry.data[date_field] : '';
    const extra = extra_field ? entry.data[extra_field] : '';
    const url = entry.url || '#';

    // Apply character limits
    if (title_limit > 0 && title.length > title_limit) {
      title = title.substring(0, title_limit) + '...';
    }
    if (excerpt_limit > 0 && excerpt.length > excerpt_limit) {
      excerpt = excerpt.substring(0, excerpt_limit) + '...';
    }

    return `
      <article class="collection-item">
        ${show_image && image ? `<div class="item-image"><img src="${image}" alt="${title}" loading="lazy"></div>` : ''}
        <div class="item-content">
          ${show_date && date ? `<time class="item-date">${date}</time>` : ''}
          ${show_title && title ? `<h3 class="item-title">${title}</h3>` : ''}
          ${show_excerpt && excerpt ? `<p class="item-excerpt">${excerpt}</p>` : ''}
          ${show_extra && extra ? `<div class="item-extra">${extra}</div>` : ''}
          ${show_read_more ? `<a href="${url}" class="item-read-more">${readMoreLabel}</a>` : ''}
        </div>
      </article>
    `;
  }).join('');

  return `
    <div class="collection-grid" style="display: grid; grid-template-columns: repeat(${columns}, 1fr); gap: 24px;">
      ${items}
    </div>
  `;
}
```
