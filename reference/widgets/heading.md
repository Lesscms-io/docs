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
| `widget` | object | Widget properties (inline-edited content, no configurable fields) |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "heading",
  "uuid": "2dbc3aaa-c75f-4141-b547-e5afb921dbb6",
  "widget": {},
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

## Usage Example

```javascript
async function renderHeading(widget, language, entryData, api) {
  const { content_source, level, collection_code, field_code, entry_id } = widget.widget;
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
    text = widget.widget?.html?.[language] || widget.widget?.html?.en || '';
  }

  const tag = `h${level || 2}`;
  return `<${tag}>${text}</${tag}>`;
}
```
