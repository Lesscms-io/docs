# Blockquote Widget

A styled quotation block with optional author attribution.

## Widget Type

```
blockquote
```

## Data Properties

| Property | Type | Global | Description |
|----------|------|--------|-------------|
| `quote` | object | No | Quote text content (multilingual) |
| `author` | object | No | Author name (multilingual) |

## Example Response

```json
{
  "widget_type": "blockquote",
  "uuid": "quote-123",
  "data": {
    "quote": {
      "en": "The only way to do great work is to love what you do.",
      "pl": "Jedynym sposobem na świetną pracę jest kochać to, co robisz."
    },
    "author": {
      "en": "Steve Jobs",
      "pl": "Steve Jobs"
    }
  },
  "settings": {
    "textAlign": "left",
    "paddingTop": 20,
    "paddingBottom": 20,
    "borderColor": "#007BFF"
  }
}
```

## Usage Example

```javascript
// Render blockquote widget
function renderBlockquote(widget, language) {
  const { quote, author } = widget.data;

  const quoteText = quote?.[language] || quote?.en || '';
  const authorText = author?.[language] || author?.en || '';

  return `
    <blockquote class="widget-blockquote">
      <p>${quoteText}</p>
      ${authorText ? `<cite>— ${authorText}</cite>` : ''}
    </blockquote>
  `;
}
```

## Styling Example

```css
.widget-blockquote {
  border-left: 4px solid #007BFF;
  padding-left: 20px;
  margin: 20px 0;
  font-style: italic;
}

.widget-blockquote cite {
  display: block;
  margin-top: 10px;
  font-style: normal;
  font-weight: 600;
  color: #666;
}
```
