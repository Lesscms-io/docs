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
| `config.layout` | string | Display layout: `"grid"`, `"list"`, `"cards"` (default: `"grid"`) |
| `config.columns` | string | Number of columns: `"1"`, `"2"`, `"3"`, `"4"` (default: `"3"`) |
| `config.posts_count` | number | Maximum entries to display (default: 6) |
| `config.order_by` | string | Order field: `"created_at"`, `"title"`, `"random"` (default: `"created_at"`) |
| `config.order_dir` | string | Order direction: `"asc"`, `"desc"` (default: `"desc"`) |
| `config.title_field` | string\|null | Field code for title |
| `config.title_limit` | number | Character limit for title (0 = no limit) |
| `config.excerpt_field` | string\|null | Field code for excerpt |
| `config.excerpt_limit` | number | Character limit for excerpt (0 = no limit) |
| `config.image_field` | string\|null | Field code for image |
| `config.date_field` | string\|null | Field code for date |
| `config.show_title` | boolean | Display title (default: true) |
| `config.show_excerpt` | boolean | Display excerpt (default: true) |
| `config.show_image` | boolean | Display image (default: true) |
| `config.show_date` | boolean | Display date (default: true) |
| `config.show_read_more` | boolean | Display read more link (default: true) |
| `config.read_more_text` | object | Multilingual read more text `{ "en": "...", "pl": "..." }` |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "collection-grid",
  "uuid": "grid-123",
  "config": {
    "collection_code": "blog",
    "layout": "grid",
    "columns": "3",
    "posts_count": 6,
    "order_by": "created_at",
    "order_dir": "desc",
    "title_field": "title",
    "title_limit": 100,
    "excerpt_field": "excerpt",
    "excerpt_limit": 200,
    "image_field": "featured_image",
    "date_field": "published_at",
    "show_title": true,
    "show_excerpt": true,
    "show_image": true,
    "show_date": true,
    "show_read_more": true,
    "read_more_text": {
      "en": "Read More",
      "pl": "Czytaj wiecej"
    }
  },
  "settings": {
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Layout Values

| Value | Description |
|-------|-------------|
| `grid` | Entries displayed in a grid |
| `list` | Entries displayed as a vertical list |
| `cards` | Entries displayed as styled cards |

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
    layout, columns,
    show_title, show_excerpt, show_image, show_date, show_read_more,
    title_field, excerpt_field, image_field, date_field,
    title_limit, excerpt_limit,
    read_more_text
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
    <div class="collection-${layout}" style="display: grid; grid-template-columns: repeat(${columns}, 1fr); gap: 24px;">
      ${items}
    </div>
  `;
}
```
