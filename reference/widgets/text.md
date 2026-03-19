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
| `widget` | object | Widget properties |
| `widget.text` | object | Text element-group |
| `widget.text.html` | object | Multilingual HTML content (`{ "en": "<p>...</p>", "pl": "<p>...</p>" }`) |
| `widget.text.color` | string\|null | Text color (CSS color or `"var:colorName"` variable reference) |
| `widget.text.color:hover` | string\|null | Text color on hover |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "text",
  "uuid": "7e9d4859-a01a-4c76-af3d-4f73209a446b",
  "widget": {
    "text": {
      "html": {
        "pl": "<h2>Lorem ipsum dolor sit amet</h2><p>Consectetur adipiscing elit.</p>",
        "en": "<h2>Lorem ipsum dolor sit amet</h2><p>Consectetur adipiscing elit.</p>"
      },
      "color": "var:dark",
      "color:hover": null
    }
  },
  "settings": {
    "paddingTop": 8,
    "paddingBottom": 8,
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

The `widget.text.html` property contains HTML-formatted rich text. Common elements include:

- `<p>` - Paragraphs
- `<h1>`-`<h6>` - Headings
- `<strong>`, `<b>` - Bold text
- `<em>`, `<i>` - Italic text
- `<a href="...">` - Links
- `<ul>`, `<ol>`, `<li>` - Lists
- `<blockquote>` - Quotes
- `<span style="...">` - Inline styling

## Color Values

The `color` and `color:hover` fields support two formats:

- **CSS color**: Any valid CSS color string (e.g. `"#333333"`, `"rgb(0,0,0)"`)
- **Variable reference**: `"var:colorName"` or `"var:colorName:opacity"` — references a project color variable (e.g. `"var:dark"`, `"var:primary:80"`)

## Usage Example

```javascript
function renderText(widget, language) {
  const text = widget.widget?.text || {};
  const html = text.html?.[language]
    || text.html?.en
    || '';

  const style = text.color ? `color: ${resolveColor(text.color)}` : '';

  return `<div class="text-widget" style="${style}">${html}</div>`;
}
```

## Sanitization

> **Important**: Always sanitize HTML content before rendering to prevent XSS attacks.

```javascript
import DOMPurify from 'dompurify';

function renderTextSafe(widget, language) {
  const html = widget.widget?.text?.html?.[language] || '';
  const sanitized = DOMPurify.sanitize(html);

  return `<div class="text-widget">${sanitized}</div>`;
}
```
