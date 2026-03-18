# Text Widget

A rich text content widget for paragraphs and formatted text.

## Widget Type

```
text
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"text"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget properties (inline-edited content, no configurable fields) |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "text",
  "uuid": "7e9d4859-a01a-4c76-af3d-4f73209a446b",
  "widget": {},
  "settings": {
    "paddingRight": 0,
    "horizontalAlign": "center",
    "responsive": {
      "tablet": {
        "paddingRight": 0,
        "horizontalAlign": "center"
      },
      "mobile": {
        "paddingRight": 0,
        "horizontalAlign": "center"
      }
    }
  }
}
```

## Content Format

The `widget.html` property contains HTML-formatted rich text. Common elements include:

- `<p>` - Paragraphs
- `<h1>`-`<h6>` - Headings
- `<strong>`, `<b>` - Bold text
- `<em>`, `<i>` - Italic text
- `<a href="...">` - Links
- `<ul>`, `<ol>`, `<li>` - Lists
- `<blockquote>` - Quotes
- `<span style="...">` - Inline styling

## Usage Example

```javascript
function renderText(widget, language) {
  const html = widget.widget?.html?.[language]
    || widget.widget?.html?.en
    || '';

  return `<div class="text-widget">${html}</div>`;
}
```

## Sanitization

> **Important**: Always sanitize HTML content before rendering to prevent XSS attacks.

```javascript
import DOMPurify from 'dompurify';

function renderTextSafe(widget, language) {
  const html = widget.widget?.html?.[language] || '';
  const sanitized = DOMPurify.sanitize(html);

  return `<div class="text-widget">${sanitized}</div>`;
}
```
