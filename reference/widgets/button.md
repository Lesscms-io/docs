# Button Widget

A call-to-action button with customizable text, link, style, and size.

## Widget Type

```
button
```

## Data Properties

| Property | Type | Global | Description |
|----------|------|--------|-------------|
| `text` | object | No | Button label text (multilingual) |
| `url` | string | Yes | Button link URL |
| `style` | string | Yes | Button style: `primary`, `secondary`, `outline` |
| `size` | string | Yes | Button size: `sm`, `md`, `lg` |
| `target_blank` | boolean | Yes | Open link in new tab |

## Example Response

```json
{
  "widget_type": "button",
  "uuid": "btn-123",
  "data": {
    "text": {
      "en": "Learn More",
      "pl": "Dowiedz się więcej"
    },
    "url": "https://example.com/about",
    "style": "primary",
    "size": "md",
    "target_blank": false
  },
  "settings": {
    "textAlign": "center",
    "marginTop": 20,
    "marginBottom": 20
  }
}
```

## Style Values

| Value | Description |
|-------|-------------|
| `primary` | Main accent color, filled background |
| `secondary` | Secondary color, filled background |
| `outline` | Transparent background with border |

## Size Values

| Value | Description |
|-------|-------------|
| `sm` | Small button (padding: 8px 16px) |
| `md` | Medium button (padding: 12px 24px) |
| `lg` | Large button (padding: 16px 32px) |

## Usage Example

```javascript
// Render button widget
function renderButton(widget, language) {
  const { text, url, style, size, target_blank } = widget.data;

  return `
    <a
      href="${url}"
      class="btn btn-${style} btn-${size}"
      ${target_blank ? 'target="_blank" rel="noopener"' : ''}
    >
      ${text[language] || text.en || ''}
    </a>
  `;
}
```
