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
| `widget.route_uuid` | string\|null | UUID of the route used for entry links |
| `widget.entry_source` | string | Entry source: `"static"` (specific entry) or `"url"` (from URL segment) (default: `"static"`) |
| `widget.entry_id` | string\|null | Specific entry ID (used when entry_source is `"static"`) |
| `widget.entry_url_segment` | number | URL segment index to resolve entry from (default: 1, used when entry_source is `"url"`) |
| `widget.entry_template` | string | Entry template identifier for rendering (default: `"default:standard"`) |
| `widget.title_field` | string\|null | Field code for title |
| `widget.content_field` | string\|null | Field code for content |
| `widget.image_field` | string\|null | Field code for image |
| `widget.show_title` | boolean | Display title (default: true) |
| `widget.show_content` | boolean | Display content (default: true) |
| `widget.show_image` | boolean | Display image (default: true) |
| `widget.use_custom_layout` | boolean | Use custom layout configuration (default: false) |
| `widget.layout_config` | object\|null | Custom layout configuration object |
| `settings` | object | Style settings (optional) |

### Server-Side Enrichment Fields

When `entry_source` is `"static"` and the API can resolve the entry server-side, the following field is conditionally included. When absent (e.g., `entry_source` is `"url"`), fetch the entry client-side via the Collection API.

| Property | Type | Description |
|----------|------|-------------|
| `widget.entry` | object | Pre-fetched entry data (included when `entry_source` is `"static"` and `entry_id` is set) |

## Example Response

```json
{
  "widget_type": "collection-single",
  "uuid": "single-123",
  "widget": {
    "collection_code": "team",
    "route_uuid": null,
    "entry_source": "static",
    "entry_id": "entry-uuid-456",
    "entry_url_segment": 1,
    "entry_template": "default:standard",
    "title_field": "name",
    "content_field": "bio",
    "image_field": "photo",
    "show_title": true,
    "show_content": true,
    "show_image": true,
    "use_custom_layout": false,
    "layout_config": null
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
