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
| `config.posts_count` | number | Maximum entries to display |
| `config.columns` | number | Number of columns |
| `config.order_by` | string | Order field |
| `config.order_dir` | string | Order direction: `"asc"`, `"desc"` |
| `config.title_field` | string | Field code for title |
| `config.excerpt_field` | string | Field code for excerpt |
| `config.image_field` | string | Field code for image |
| `config.date_field` | string | Field code for date |
| `config.show_title` | boolean | Display title |
| `config.show_excerpt` | boolean | Display excerpt |
| `config.show_image` | boolean | Display image |
| `config.show_date` | boolean | Display date |
| `config.show_read_more` | boolean | Display read more link |
| `config.link_field` | string | Field for entry URL |
| `content` | object | Widget content |
| `content.read_more_text` | object | Multilingual read more text |
| `data` | object | Fetched entries |
| `data.entries` | array | Array of collection entries |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "collection-grid",
  "uuid": "grid-123",
  "config": {
    "collection_code": "blog",
    "posts_count": 6,
    "columns": 3,
    "order_by": "created_at",
    "order_dir": "desc",
    "title_field": "title",
    "excerpt_field": "excerpt",
    "image_field": "featured_image",
    "date_field": "published_at",
    "show_title": true,
    "show_excerpt": true,
    "show_image": true,
    "show_date": true,
    "show_read_more": true,
    "link_field": "url"
  },
  "content": {
    "read_more_text": {
      "en": "Read More",
      "pl": "Czytaj więcej"
    }
  },
  "data": {
    "entries": [
      {
        "uuid": "entry-1",
        "url": "/blog/my-post",
        "data": {
          "title": { "en": "My Blog Post", "pl": "Mój wpis" },
          "excerpt": { "en": "This is an excerpt...", "pl": "To jest skrót..." },
          "featured_image": "https://cdn.example.com/img.jpg",
          "published_at": "2024-01-15"
        }
      }
    ]
  },
  "settings": {
    "paddingTop": 40,
    "paddingBottom": 40,
    "responsive": {
      "tablet": { "columns": 2 },
      "mobile": { "columns": 1 }
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
  const { columns, show_title, show_excerpt, show_image, show_date, show_read_more, link_field } = widget.config;
  const entries = widget.data?.entries || [];
  const readMoreText = widget.content?.read_more_text?.[language] || 'Read More';

  if (entries.length === 0) return '<p>No entries found.</p>';

  const items = entries.map(entry => {
    const title = entry.data[widget.config.title_field]?.[language] || '';
    const excerpt = entry.data[widget.config.excerpt_field]?.[language] || '';
    const image = entry.data[widget.config.image_field] || '';
    const date = entry.data[widget.config.date_field] || '';
    const url = entry.url || entry.data[link_field] || '#';

    return `
      <article class="collection-item">
        ${show_image && image ? `<div class="item-image"><img src="${image}" alt="${title}" loading="lazy"></div>` : ''}
        <div class="item-content">
          ${show_date && date ? `<time class="item-date">${date}</time>` : ''}
          ${show_title && title ? `<h3 class="item-title">${title}</h3>` : ''}
          ${show_excerpt && excerpt ? `<p class="item-excerpt">${excerpt}</p>` : ''}
          ${show_read_more ? `<a href="${url}" class="item-read-more">${readMoreText}</a>` : ''}
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
