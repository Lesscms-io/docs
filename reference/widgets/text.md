# Text Widget

A rich text content widget for paragraphs and formatted text.

## Widget Type

```
text
```

## Data Properties

| Property | Type | Global | Description |
|----------|------|--------|-------------|
| `content` | object | No | Rich text content (multilingual, HTML) |

## Example Response

```json
{
  "widget_type": "text",
  "uuid": "text-123",
  "data": {
    "content": {
      "en": "<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p><p>Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris.</p>",
      "pl": "<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p><p>Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris.</p>"
    }
  },
  "settings": {
    "textAlign": "left",
    "paddingTop": 16,
    "paddingBottom": 16
  }
}
```

## Content Format

The `content` property contains HTML-formatted rich text. Common elements include:

- `<p>` - Paragraphs
- `<strong>`, `<b>` - Bold text
- `<em>`, `<i>` - Italic text
- `<a href="...">` - Links
- `<ul>`, `<ol>`, `<li>` - Lists
- `<blockquote>` - Quotes

## Usage Example

```javascript
// Render text widget
function renderText(widget, language) {
  const content = widget.data.content?.[language]
    || widget.data.content?.en
    || '';

  return `<div class="text-widget">${content}</div>`;
}
```

## Sanitization

> **Important**: Always sanitize HTML content before rendering to prevent XSS attacks.

```javascript
import DOMPurify from 'dompurify';

function renderTextSafe(widget, language) {
  const content = widget.data.content?.[language] || '';
  const sanitized = DOMPurify.sanitize(content);

  return `<div class="text-widget">${sanitized}</div>`;
}
```
