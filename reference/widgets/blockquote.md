# Blockquote Widget

A styled quotation block with optional author attribution and dynamic content support.

## Widget Type

```
blockquote
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"blockquote"` |
| `uuid` | string | Unique widget identifier |
| `config` | object | Widget configuration |
| `config.content_source` | string | `"static"` or `"dynamic"` |
| `config.collection_code` | string | Collection code (when dynamic) |
| `config.field_code` | string | Field code (when dynamic) |
| `config.entry_id` | string | Entry ID (when dynamic) |
| `content` | object | Widget content (when static) |
| `content.text` | object | Multilingual quote text |
| `content.author` | object | Multilingual author name |
| `settings` | object | Style settings (optional) |

## Example Response (Static)

```json
{
  "widget_type": "blockquote",
  "uuid": "quote-123",
  "config": {
    "content_source": "static"
  },
  "content": {
    "text": {
      "en": "The only way to do great work is to love what you do.",
      "pl": "Jedynym sposobem na świetną pracę jest kochać to, co robisz."
    },
    "author": {
      "en": "Steve Jobs",
      "pl": "Steve Jobs"
    }
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

## Example Response (Dynamic)

```json
{
  "widget_type": "blockquote",
  "uuid": "quote-456",
  "config": {
    "content_source": "dynamic",
    "collection_code": "testimonials",
    "field_code": "quote",
    "entry_id": "testimonial-1"
  },
  "settings": {}
}
```

## Usage Example

```javascript
function renderBlockquote(widget, language) {
  const { text, author } = widget.content || {};

  const quoteText = text?.[language] || text?.en || '';
  const authorText = author?.[language] || author?.en || '';

  return `
    <blockquote class="widget-blockquote">
      <p>${quoteText}</p>
      ${authorText ? `<cite>— ${authorText}</cite>` : ''}
    </blockquote>
  `;
}
```
