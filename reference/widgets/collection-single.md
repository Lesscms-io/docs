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
| `widget` | object | Widget data |
| `widget.collection_code` | string\|null | Collection code |
| `widget.entry_id` | string\|null | Specific entry ID |
| `widget.layout` | string | Display layout: `"standard"`, `"side-by-side"`, `"hero"` (default: `"standard"`) |
| `widget.title_field` | string\|null | Field code for title |
| `widget.content_field` | string\|null | Field code for content |
| `widget.image_field` | string\|null | Field code for image |
| `widget.show_title` | boolean | Display title (default: true) |
| `widget.show_content` | boolean | Display content (default: true) |
| `widget.show_image` | boolean | Display image (default: true) |
| `widget.route_uuid` | string\|null | Route UUID for entry detail pages |
| `widget.use_custom_layout` | boolean | Use custom entry layout template |
| `widget.layout_config` | object\|null | Custom layout configuration |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "collection-single",
  "uuid": "single-123",
  "widget": {
    "collection_code": "team",
    "entry_id": "entry-uuid-456",
    "layout": "standard",
    "title_field": "name",
    "content_field": "bio",
    "image_field": "photo",
    "show_title": true,
    "show_content": true,
    "show_image": true,
    "route_uuid": null,
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

## Usage Example

```javascript
function renderCollectionSingle(widget, language) {
  const {
    entry_template, show_title, show_content, show_image,
    title_field, content_field, image_field
  } = widget.widget;

  // Entry may be pre-fetched by the API (server-side enrichment) when entry_source is "static".
  // If widget.entry exists, use it directly. Otherwise, fetch client-side.
  const entry = widget.widget.entry || null; // Fallback: fetch from /api/:project_code/collections/:collection_code/:entry_id
  if (!entry) return '<p>Entry not found.</p>';

  const title = title_field ? (entry.data[title_field]?.[language] || '') : '';
  const content = content_field ? (entry.data[content_field]?.[language] || '') : '';
  const image = image_field ? entry.data[image_field] : '';

  return `
    <article class="collection-single template-${entry_template}">
      ${show_image && image ? `<div class="single-image"><img src="${image}" alt="${title}"></div>` : ''}
      <div class="single-content">
        ${show_title && title ? `<h2 class="single-title">${title}</h2>` : ''}
        ${show_content && content ? `<div class="single-body">${content}</div>` : ''}
      </div>
    </article>
  `;
}
```
