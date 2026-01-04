# Heading Widget

A text heading widget supporting h1-h6 levels with optional dynamic content from collections.

## Widget Type

```
heading
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"heading"` |
| `uuid` | string | Unique widget identifier |
| `config` | object | Widget configuration |
| `config.content_source` | string | `"static"` or `"dynamic"` |
| `config.level` | number | Heading level: 1-6 (h1-h6) |
| `config.collection_code` | string | Collection code (when dynamic) |
| `config.field_code` | string | Field to display (when dynamic) |
| `config.entry_id` | string | Specific entry ID (when dynamic + static entry) |
| `content` | object | Widget content (when static) |
| `content.html` | object | Multilingual HTML content |
| `settings` | object | Style settings (optional) |

## Example Response (Static)

```json
{
  "widget_type": "heading",
  "uuid": "2dbc3aaa-c75f-4141-b547-e5afb921dbb6",
  "config": {
    "content_source": "static",
    "level": 2
  },
  "content": {
    "html": {
      "en": "<h2>Welcome to Our Website</h2>",
      "pl": "<h2><strong>USŁUGI MARKETINGOWE</strong></h2>"
    }
  },
  "settings": {
    "horizontalAlign": "center",
    "responsive": {
      "tablet": {
        "horizontalAlign": "center"
      },
      "mobile": {
        "horizontalAlign": "center"
      }
    }
  }
}
```

## Example Response (Dynamic)

```json
{
  "widget_type": "heading",
  "uuid": "heading-456",
  "config": {
    "content_source": "dynamic",
    "level": 1,
    "collection_code": "blog",
    "field_code": "title",
    "entry_id": null
  },
  "settings": {
    "horizontalAlign": "left",
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Content Source

| Value | Description |
|-------|-------------|
| `static` | Use the `content.html` property directly |
| `dynamic` | Fetch content from a collection field |

## Usage Example

```javascript
async function renderHeading(widget, language, entryData, api) {
  const { content_source, level, collection_code, field_code, entry_id } = widget.config;
  let text = '';

  if (content_source === 'dynamic') {
    // For dynamic content, fetch from collection or use provided entry data
    if (entryData) {
      text = entryData[field_code]?.[language] || entryData[field_code]?.en || '';
    } else if (collection_code && entry_id) {
      const entry = await api.getEntry(collection_code, entry_id);
      text = entry.data[field_code]?.[language] || '';
    }
  } else {
    // Static content from widget
    text = widget.content?.html?.[language] || widget.content?.html?.en || '';
  }

  const tag = `h${level || 2}`;
  return `<${tag}>${text}</${tag}>`;
}
```
