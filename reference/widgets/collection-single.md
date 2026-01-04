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
| `config.collection_code` | string\|null | Collection code |
| `config.route_uuid` | string\|null | Route UUID for link generation |
| `config.entry_source` | string | Entry source: `"static"` or `"url"` (default: `"static"`) |
| `config.entry_id` | string\|null | Specific entry ID (when `entry_source` is `"static"`) |
| `config.entry_url_segment` | number | URL segment index (when `entry_source` is `"url"`, default: 1) |
| `config.entry_template` | string | Template reference (default: `"default:standard"`) |
| `config.title_field` | string\|null | Field code for title |
| `config.content_field` | string\|null | Field code for content |
| `config.image_field` | string\|null | Field code for image |
| `config.show_title` | boolean | Display title (default: true) |
| `config.show_content` | boolean | Display content (default: true) |
| `config.show_image` | boolean | Display image (default: true) |
| `config.use_custom_layout` | boolean | Use custom layout (legacy, default: false) |
| `config.layout_config` | object\|null | Custom layout configuration (legacy) |
| `settings` | object | Style settings (optional) |

## Example Response (Static Entry)

```json
{
  "widget_type": "collection-single",
  "uuid": "single-123",
  "config": {
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

## Example Response (URL Entry)

```json
{
  "widget_type": "collection-single",
  "uuid": "single-456",
  "config": {
    "collection_code": "blog",
    "route_uuid": "route-uuid-789",
    "entry_source": "url",
    "entry_id": null,
    "entry_url_segment": 2,
    "entry_template": "custom:792b14fe-cd64-454c-933a-02e0718b3f3e",
    "title_field": "title",
    "content_field": "content",
    "image_field": "featured_image",
    "show_title": true,
    "show_content": true,
    "show_image": true,
    "use_custom_layout": false,
    "layout_config": null
  },
  "settings": {}
}
```

## Entry Source Values

| Value | Description |
|-------|-------------|
| `static` | Use specific `entry_id` |
| `url` | Extract entry from URL segment at `entry_url_segment` index |

## Entry Template

The `entry_template` field supports:

| Value | Description |
|-------|-------------|
| `"default:standard"` | Use default rendering with field mappings |
| `"custom:uuid"` | Use custom template defined in collection settings |

When using a custom template, the `title_field`, `content_field`, `image_field` mappings are ignored.

## Usage Example

```javascript
function renderCollectionSingle(widget, language) {
  const {
    entry_source, entry_id, entry_url_segment,
    entry_template,
    show_title, show_content, show_image,
    title_field, content_field, image_field
  } = widget.config;

  // If using custom template, render differently
  if (entry_template && entry_template.startsWith('custom:')) {
    return renderWithTemplate(widget, entry_template, language);
  }

  const entry = widget.data?.entry;

  if (!entry) return '<p>Entry not found.</p>';

  const title = title_field ? (entry.data[title_field]?.[language] || '') : '';
  const content = content_field ? (entry.data[content_field]?.[language] || '') : '';
  const image = image_field ? entry.data[image_field] : '';

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
