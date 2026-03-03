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
| `config.collection_code` | string\|null | Collection code to display |
| `config.route_uuid` | string\|null | UUID of the route used for entry links |
| `config.columns` | number | Number of columns (default: 3) |
| `config.columns_tablet` | number\|null | Number of columns on tablet breakpoint |
| `config.columns_mobile` | number\|null | Number of columns on mobile breakpoint |
| `config.gap` | number | Grid gap in pixels (default: 16) |
| `config.posts_count` | number | Maximum entries to display (default: 6) |
| `config.card_style` | string | Card display style (default: `"default"`) |
| `config.entry_template` | string\|null | Entry template identifier for custom rendering |
| `config.order_by` | string | Order field: `"created_at"`, `"title"`, `"random"` (default: `"created_at"`) |
| `config.order_dir` | string | Order direction: `"asc"`, `"desc"` (default: `"desc"`) |
| `config.title_field` | string\|null | Field code for title |
| `config.title_limit` | number | Character limit for title (0 = no limit) |
| `config.excerpt_field` | string\|null | Field code for excerpt |
| `config.excerpt_limit` | number | Character limit for excerpt (0 = no limit) |
| `config.image_field` | string\|null | Field code for image |
| `config.date_field` | string\|null | Field code for date |
| `config.extra_field` | string\|null | Field code for an additional display field |
| `config.tags_field` | string\|null | Field code for tags |
| `config.show_title` | boolean | Display title (default: true) |
| `config.show_excerpt` | boolean | Display excerpt (default: true) |
| `config.show_image` | boolean | Display image (default: true) |
| `config.show_date` | boolean | Display date (default: true) |
| `config.show_read_more` | boolean | Display read more link (default: true) |
| `config.show_pagination` | boolean | Display pagination controls (default: false) |
| `config.show_extra` | boolean | Display extra field (default: false) |
| `config.show_tags` | boolean | Display tags (default: false) |
| `config.read_more_text` | object | Multilingual read more text `{ "en": "...", "pl": "..." }` |
| `config.exclude_current_entry` | boolean | Exclude current entry from results (useful on detail pages) |
| `config.filter_field` | string\|null | Field code to filter entries by |
| `config.filter_source` | string | Filter value source: `"static"`, `"url"` (default: `"static"`) |
| `config.filter_value` | string\|null | Static filter value |
| `config.filter_url_segment` | number\|null | URL segment index to use as filter value |
| `config.card_background_color` | string\|null | Card background color (hex or `var:*`) |
| `config.card_text_color` | string\|null | Card text color (hex or `var:*`) |
| `config.card_border_radius` | number\|null | Card border radius in pixels |
| `config.card_padding` | number\|null | Card inner padding in pixels |
| `config.content_gap` | number | Gap between content elements in pixels (default: 8) |
| `config.field_order` | array\|null | Order of displayed fields (managed via drag & drop in CMS) |
| `config.use_custom_layout` | boolean | Use custom layout configuration (default: false) |
| `config.layout_config` | object\|null | Custom layout configuration object |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "collection-grid",
  "uuid": "grid-123",
  "config": {
    "collection_code": "blog",
    "route_uuid": "route-uuid-789",
    "columns": 3,
    "columns_tablet": 2,
    "columns_mobile": 1,
    "gap": 24,
    "posts_count": 6,
    "card_style": "default",
    "entry_template": null,
    "order_by": "created_at",
    "order_dir": "desc",
    "title_field": "title",
    "title_limit": 100,
    "excerpt_field": "excerpt",
    "excerpt_limit": 200,
    "image_field": "featured_image",
    "date_field": "published_at",
    "extra_field": null,
    "tags_field": "tags",
    "show_title": true,
    "show_excerpt": true,
    "show_image": true,
    "show_date": true,
    "show_read_more": true,
    "show_pagination": false,
    "show_extra": false,
    "show_tags": true,
    "read_more_text": {
      "en": "Read More",
      "pl": "Czytaj wiecej"
    },
    "exclude_current_entry": false,
    "filter_field": null,
    "filter_source": "static",
    "filter_value": null,
    "filter_url_segment": null,
    "card_background_color": "#ffffff",
    "card_text_color": null,
    "card_border_radius": 8,
    "card_padding": 16,
    "content_gap": 8,
    "field_order": ["image", "date", "title", "excerpt", "tags", "read_more"],
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

## Order Options

| order_by | Description |
|----------|-------------|
| `created_at` | Order by creation date |
| `title` | Order alphabetically by title |
| `random` | Random order |

## Usage Example

```javascript
function renderCollectionGrid(widget, language) {
  const {
    columns,
    show_title, show_excerpt, show_image, show_date, show_read_more,
    title_field, excerpt_field, image_field, date_field,
    title_limit, excerpt_limit,
    read_more_text, gap
  } = widget.config;

  const entries = widget.data?.entries || [];
  const readMoreLabel = read_more_text?.[language] || 'Read More';

  if (entries.length === 0) return '<p>No entries found.</p>';

  const items = entries.map(entry => {
    let title = title_field ? (entry.data[title_field]?.[language] || '') : '';
    let excerpt = excerpt_field ? (entry.data[excerpt_field]?.[language] || '') : '';
    const image = image_field ? entry.data[image_field] : '';
    const date = date_field ? entry.data[date_field] : '';
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
          ${show_read_more ? `<a href="${url}" class="item-read-more">${readMoreLabel}</a>` : ''}
        </div>
      </article>
    `;
  }).join('');

  return `
    <div class="collection-grid" style="display: grid; grid-template-columns: repeat(${columns}, 1fr); gap: ${gap}px;">
      ${items}
    </div>
  `;
}
```
