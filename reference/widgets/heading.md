# Heading Widget

A text heading widget supporting h1-h6 levels with optional dynamic content from collections.

## Widget Type

```
heading
```

## Data Properties

| Property | Type | Global | Description |
|----------|------|--------|-------------|
| `text` | object | No | Heading text content (multilingual, rich text) |
| `level` | number | Yes | Heading level: 1-6 (h1-h6) |
| `content_source` | string | Yes | Content source: `static` or `dynamic` |
| `collection_code` | string | Yes | Collection code (when dynamic) |
| `field_code` | string | Yes | Field to display (when dynamic) |
| `entry_source` | string | Yes | Entry source: `static` or `url` |
| `entry_id` | string | Yes | Specific entry ID (when static) |
| `entry_url_segment` | number | Yes | URL segment index for entry (when url) |

## Example Response (Static)

```json
{
  "widget_type": "heading",
  "uuid": "heading-123",
  "data": {
    "text": {
      "en": "<h2>Welcome to Our Website</h2>",
      "pl": "<h2>Witamy na naszej stronie</h2>"
    },
    "level": 2,
    "content_source": "static"
  },
  "settings": {
    "textAlign": "center",
    "marginBottom": 24
  }
}
```

## Example Response (Dynamic)

```json
{
  "widget_type": "heading",
  "uuid": "heading-456",
  "data": {
    "content_source": "dynamic",
    "collection_code": "blog",
    "field_code": "title",
    "entry_source": "url",
    "entry_url_segment": 1,
    "level": 1
  },
  "settings": {
    "textAlign": "left"
  }
}
```

## Content Source

| Value | Description |
|-------|-------------|
| `static` | Use the `text` property directly |
| `dynamic` | Fetch content from a collection field |

## Entry Source (for dynamic content)

| Value | Description |
|-------|-------------|
| `static` | Use specific `entry_id` |
| `url` | Get entry ID from URL segment |

## Usage Example

```javascript
// Render heading widget
async function renderHeading(widget, language, urlSegments, api) {
  const { content_source, text, level } = widget.data;
  let content = '';

  if (content_source === 'dynamic') {
    const { collection_code, field_code, entry_source, entry_id, entry_url_segment } = widget.data;

    // Get entry ID from URL or static
    const entryId = entry_source === 'url'
      ? urlSegments[entry_url_segment - 1]
      : entry_id;

    // Fetch entry from API
    const entry = await api.getEntry(collection_code, entryId);
    content = entry.data[field_code]?.[language] || '';
  } else {
    content = text?.[language] || text?.en || '';
  }

  const tag = `h${level || 2}`;
  return `<${tag}>${content}</${tag}>`;
}
```
