# Collection Single Widget

Display a single entry from a collection with optional template-based rendering.

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
| `config.collection_code` | string | Collection code |
| `config.entry_source` | string | `"static"` or `"url"` |
| `config.entry_id` | string | Specific entry ID (when static) |
| `config.entry_url_segment` | number | URL segment index (when url) |
| `config.template_code` | string | Template code to use (optional) |
| `config.title_field` | string | Field code for title |
| `config.content_field` | string | Field code for content |
| `config.image_field` | string | Field code for image |
| `config.link_field` | string | Field for entry URL |
| `config.show_title` | boolean | Display title |
| `config.show_content` | boolean | Display content |
| `config.show_image` | boolean | Display image |
| `data` | object | Fetched entry data |
| `data.entry` | object | Entry object with data |
| `settings` | object | Style settings (optional) |

## Example Response (Static Entry)

```json
{
  "widget_type": "collection-single",
  "uuid": "single-123",
  "config": {
    "collection_code": "team",
    "entry_source": "static",
    "entry_id": "entry-uuid-456",
    "entry_url_segment": null,
    "template_code": null,
    "title_field": "name",
    "content_field": "bio",
    "image_field": "photo",
    "link_field": "url",
    "show_title": true,
    "show_content": true,
    "show_image": true
  },
  "data": {
    "entry": {
      "uuid": "entry-uuid-456",
      "url": "/team/john-doe",
      "data": {
        "name": { "en": "John Doe", "pl": "John Doe" },
        "bio": { "en": "Software engineer...", "pl": "Inżynier oprogramowania..." },
        "photo": "https://cdn.example.com/john.jpg"
      }
    }
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

## Example Response (URL Entry)

```json
{
  "widget_type": "collection-single",
  "uuid": "single-456",
  "config": {
    "collection_code": "blog",
    "entry_source": "url",
    "entry_id": null,
    "entry_url_segment": 2,
    "template_code": "blog-detail",
    "title_field": "title",
    "content_field": "content",
    "image_field": "featured_image",
    "link_field": "url",
    "show_title": true,
    "show_content": true,
    "show_image": true
  },
  "settings": {}
}
```

## Entry Source Values

| Value | Description |
|-------|-------------|
| `static` | Use specific `entry_id` |
| `url` | Extract entry from URL segment |

## Usage Example

```javascript
function renderCollectionSingle(widget, language) {
  const { show_title, show_content, show_image, title_field, content_field, image_field } = widget.config;
  const entry = widget.data?.entry;

  if (!entry) return '<p>Entry not found.</p>';

  const title = entry.data[title_field]?.[language] || '';
  const content = entry.data[content_field]?.[language] || '';
  const image = entry.data[image_field] || '';

  return `
    <article class="collection-single">
      ${show_image && image ? `<div class="single-image"><img src="${image}" alt="${title}"></div>` : ''}
      <div class="single-content">
        ${show_title && title ? `<h2 class="single-title">${title}</h2>` : ''}
        ${show_content && content ? `<div class="single-body">${content}</div>` : ''}
      </div>
    </article>
  `;
}
```
