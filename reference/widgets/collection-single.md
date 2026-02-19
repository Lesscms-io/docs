# Collection Single Widget

Display a single entry from a collection.

## Widget Type

```
collection-single
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"collection-single"` |
| `uuid` | string | Unique widget identifier |
| `config` | object | Widget configuration |
| `config.collection_code` | string\|null | Collection code |
| `config.entry_id` | string\|null | Specific entry ID |
| `config.layout` | string | Display layout: `"standard"`, `"card"`, `"full"` (default: `"standard"`) |
| `config.title_field` | string\|null | Field code for title |
| `config.content_field` | string\|null | Field code for content |
| `config.image_field` | string\|null | Field code for image |
| `config.show_title` | boolean | Display title (default: true) |
| `config.show_content` | boolean | Display content (default: true) |
| `config.show_image` | boolean | Display image (default: true) |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "collection-single",
  "uuid": "single-123",
  "config": {
    "collection_code": "team",
    "entry_id": "entry-uuid-456",
    "layout": "standard",
    "title_field": "name",
    "content_field": "bio",
    "image_field": "photo",
    "show_title": true,
    "show_content": true,
    "show_image": true
  },
  "settings": {
    "horizontalAlign": "center",
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
| `standard` | Standard layout with image, title, and content |
| `card` | Card-style layout with shadow and border |
| `full` | Full-width layout |

## Usage Example

```javascript
function renderCollectionSingle(widget, language) {
  const {
    layout, show_title, show_content, show_image,
    title_field, content_field, image_field
  } = widget.config;

  const entry = widget.data?.entry;
  if (!entry) return '<p>Entry not found.</p>';

  const title = title_field ? (entry.data[title_field]?.[language] || '') : '';
  const content = content_field ? (entry.data[content_field]?.[language] || '') : '';
  const image = image_field ? entry.data[image_field] : '';

  return `
    <article class="collection-single layout-${layout}">
      ${show_image && image ? `<div class="single-image"><img src="${image}" alt="${title}"></div>` : ''}
      <div class="single-content">
        ${show_title && title ? `<h2 class="single-title">${title}</h2>` : ''}
        ${show_content && content ? `<div class="single-body">${content}</div>` : ''}
      </div>
    </article>
  `;
}
```
