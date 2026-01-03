# Collection Grid Widget

A grid display of collection entries with customizable layout and fields.

## Widget Type

```
collection-grid
```

## Data Properties

| Property | Type | Global | Description |
|----------|------|--------|-------------|
| `collection_code` | string | Yes | Collection code to display |
| `layout` | string | Yes | Layout style: `grid`, `list`, `cards` |
| `columns` | string | Yes | Number of columns: `1`, `2`, `3`, `4` |
| `posts_count` | number | Yes | Maximum entries to display |
| `order_by` | string | Yes | Order field: `created_at`, `title`, `random` |
| `order_dir` | string | Yes | Order direction: `asc`, `desc` |
| `title_field` | string | Yes | Field code for entry title |
| `title_limit` | number | Yes | Character limit for title |
| `excerpt_field` | string | Yes | Field code for excerpt/description |
| `excerpt_limit` | number | Yes | Character limit for excerpt |
| `image_field` | string | Yes | Field code for featured image |
| `date_field` | string | Yes | Field code for date |
| `show_title` | boolean | Yes | Display title |
| `show_excerpt` | boolean | Yes | Display excerpt |
| `show_image` | boolean | Yes | Display image |
| `show_date` | boolean | Yes | Display date |
| `show_read_more` | boolean | Yes | Display read more link |
| `read_more_text` | object | No | Read more button text (multilingual) |

## Example Response

```json
{
  "widget_type": "collection-grid",
  "uuid": "grid-123",
  "data": {
    "collection_code": "blog",
    "layout": "cards",
    "columns": "3",
    "posts_count": 6,
    "order_by": "created_at",
    "order_dir": "desc",
    "title_field": "title",
    "title_limit": 100,
    "excerpt_field": "excerpt",
    "excerpt_limit": 150,
    "image_field": "featured_image",
    "date_field": "published_at",
    "show_title": true,
    "show_excerpt": true,
    "show_image": true,
    "show_date": true,
    "show_read_more": true,
    "read_more_text": {
      "en": "Read More",
      "pl": "Czytaj więcej"
    }
  },
  "settings": {
    "paddingTop": 40,
    "paddingBottom": 40
  }
}
```

## Layout Values

| Value | Description |
|-------|-------------|
| `grid` | Simple grid layout |
| `list` | Horizontal list with image on side |
| `cards` | Card-style layout with shadow |

## Order Options

| order_by | Description |
|----------|-------------|
| `created_at` | Order by creation date |
| `title` | Order alphabetically by title |
| `random` | Random order |

## Usage Example

```javascript
// Render collection grid widget
async function renderCollectionGrid(widget, language, api) {
  const {
    collection_code, layout, columns, posts_count,
    order_by, order_dir,
    title_field, title_limit, excerpt_field, excerpt_limit,
    image_field, date_field,
    show_title, show_excerpt, show_image, show_date, show_read_more,
    read_more_text
  } = widget.data;

  // Fetch entries from API
  const entries = await api.getCollectionEntries(collection_code, {
    limit: posts_count,
    order_by,
    order_dir
  });

  if (!entries || entries.length === 0) {
    return '<p>No entries found.</p>';
  }

  const items = entries.map(entry => {
    const title = truncate(entry.data[title_field]?.[language] || '', title_limit);
    const excerpt = truncate(entry.data[excerpt_field]?.[language] || '', excerpt_limit);
    const image = entry.data[image_field]?.url || '';
    const date = entry.data[date_field] ? formatDate(entry.data[date_field]) : '';
    const readMoreLabel = read_more_text?.[language] || read_more_text?.en || 'Read More';

    return `
      <article class="collection-item">
        ${show_image && image ? `
          <div class="item-image">
            <img src="${image}" alt="${title}" loading="lazy">
          </div>
        ` : ''}
        <div class="item-content">
          ${show_date && date ? `<time class="item-date">${date}</time>` : ''}
          ${show_title && title ? `<h3 class="item-title">${title}</h3>` : ''}
          ${show_excerpt && excerpt ? `<p class="item-excerpt">${excerpt}</p>` : ''}
          ${show_read_more ? `
            <a href="${entry.url}" class="item-read-more">${readMoreLabel}</a>
          ` : ''}
        </div>
      </article>
    `;
  }).join('');

  return `
    <div class="collection-grid layout-${layout}" style="
      display: grid;
      grid-template-columns: repeat(${columns}, 1fr);
      gap: 24px;
    ">
      ${items}
    </div>
  `;
}

function truncate(text, limit) {
  if (!limit || text.length <= limit) return text;
  return text.substring(0, limit).trim() + '...';
}

function formatDate(dateString) {
  return new Date(dateString).toLocaleDateString();
}
```

## CSS Example

```css
.collection-grid {
  display: grid;
  gap: 24px;
}

/* Cards Layout */
.layout-cards .collection-item {
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  overflow: hidden;
}

.layout-cards .item-image img {
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.layout-cards .item-content {
  padding: 20px;
}

/* List Layout */
.layout-list {
  grid-template-columns: 1fr !important;
}

.layout-list .collection-item {
  display: flex;
  gap: 24px;
}

.layout-list .item-image {
  flex: 0 0 300px;
}

.layout-list .item-image img {
  width: 100%;
  height: 200px;
  object-fit: cover;
  border-radius: 8px;
}

/* Common */
.item-date {
  font-size: 12px;
  color: #666;
  text-transform: uppercase;
}

.item-title {
  font-size: 20px;
  margin: 8px 0;
}

.item-excerpt {
  color: #666;
  line-height: 1.6;
}

.item-read-more {
  display: inline-block;
  margin-top: 12px;
  color: #007BFF;
  font-weight: 500;
}

/* Responsive */
@media (max-width: 768px) {
  .collection-grid {
    grid-template-columns: 1fr !important;
  }

  .layout-list .collection-item {
    flex-direction: column;
  }

  .layout-list .item-image {
    flex: none;
  }
}
```
