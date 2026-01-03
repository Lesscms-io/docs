# Collection Single Widget

Display a single entry from a collection.

## Widget Type

```
collection-single
```

## Data Properties

| Property | Type | Global | Description |
|----------|------|--------|-------------|
| `collection_code` | string | Yes | Collection code |
| `entry_id` | string | Yes | Specific entry ID to display |
| `layout` | string | Yes | Layout: `standard`, `card`, `full` |
| `title_field` | string | Yes | Field code for title |
| `content_field` | string | Yes | Field code for main content |
| `image_field` | string | Yes | Field code for image |
| `show_title` | boolean | Yes | Display title |
| `show_content` | boolean | Yes | Display content |
| `show_image` | boolean | Yes | Display image |

## Example Response

```json
{
  "widget_type": "collection-single",
  "uuid": "single-123",
  "data": {
    "collection_code": "team",
    "entry_id": "entry-uuid-456",
    "layout": "card",
    "title_field": "name",
    "content_field": "bio",
    "image_field": "photo",
    "show_title": true,
    "show_content": true,
    "show_image": true
  },
  "settings": {
    "textAlign": "center",
    "maxWidth": 600
  }
}
```

## Layout Values

| Value | Description |
|-------|-------------|
| `standard` | Simple layout with stacked elements |
| `card` | Card layout with shadow and padding |
| `full` | Full-width layout with image header |

## Usage Example

```javascript
// Render collection single widget
async function renderCollectionSingle(widget, language, api) {
  const {
    collection_code, entry_id, layout,
    title_field, content_field, image_field,
    show_title, show_content, show_image
  } = widget.data;

  if (!entry_id) {
    return '';
  }

  // Fetch entry from API
  const entry = await api.getEntry(collection_code, entry_id);

  if (!entry) {
    return '<p>Entry not found.</p>';
  }

  const title = entry.data[title_field]?.[language] || '';
  const content = entry.data[content_field]?.[language] || '';
  const image = entry.data[image_field]?.url || '';

  return `
    <article class="collection-single layout-${layout}">
      ${show_image && image ? `
        <div class="single-image">
          <img src="${image}" alt="${title}">
        </div>
      ` : ''}
      <div class="single-content">
        ${show_title && title ? `<h2 class="single-title">${title}</h2>` : ''}
        ${show_content && content ? `<div class="single-body">${content}</div>` : ''}
      </div>
    </article>
  `;
}
```

## CSS Example

```css
/* Standard Layout */
.layout-standard .single-image img {
  width: 100%;
  max-width: 400px;
  border-radius: 8px;
}

.layout-standard .single-content {
  margin-top: 24px;
}

/* Card Layout */
.layout-card {
  background: white;
  border-radius: 12px;
  box-shadow: 0 4px 16px rgba(0,0,0,0.1);
  overflow: hidden;
  max-width: 400px;
  margin: 0 auto;
}

.layout-card .single-image img {
  width: 100%;
  height: 250px;
  object-fit: cover;
}

.layout-card .single-content {
  padding: 24px;
  text-align: center;
}

.layout-card .single-title {
  margin: 0 0 12px;
}

.layout-card .single-body {
  color: #666;
  line-height: 1.6;
}

/* Full Layout */
.layout-full .single-image {
  position: relative;
  height: 400px;
  margin-bottom: 32px;
}

.layout-full .single-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.layout-full .single-title {
  font-size: 36px;
  margin-bottom: 24px;
}

.layout-full .single-body {
  max-width: 800px;
  margin: 0 auto;
  font-size: 18px;
  line-height: 1.8;
}
```
