# Blockquote Widget

A quote/citation with author and source attribution.

## Widget Type

```
blockquote
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"blockquote"` |
| `uuid` | string | Unique widget identifier |
| `content` | object | Widget content |
| `content.quote` | object | Multilingual quote text |
| `content.author` | object | Multilingual author name |
| `content.source` | object | Multilingual source/publication |
| `config` | object | Widget configuration |
| `config.style` | string | Quote style: `"simple"`, `"bordered"`, `"filled"` |
| `config.accent_color` | string\|null | Accent color for border/icon |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "blockquote",
  "uuid": "blockquote-123",
  "content": {
    "quote": {
      "en": "The only way to do great work is to love what you do.",
      "pl": "Jedynym sposobem na wykonanie świetnej pracy jest kochanie tego, co się robi."
    },
    "author": {
      "en": "Steve Jobs",
      "pl": "Steve Jobs"
    },
    "source": {
      "en": "Stanford Commencement Speech, 2005",
      "pl": "Przemówienie na Stanford, 2005"
    }
  },
  "config": {
    "style": "bordered",
    "accent_color": "#50a5f1"
  }
}
```

## Quote Styles

| Value | Description |
|-------|-------------|
| `simple` | Plain quote without decoration |
| `bordered` | Left border accent |
| `filled` | Background-filled box |

## Usage Example

```javascript
function renderBlockquote(widget, language) {
  const { quote, author, source } = widget.content;
  const { style, accent_color } = widget.config;

  const quoteText = quote?.[language] || quote?.en || '';
  const authorText = author?.[language] || author?.en || '';
  const sourceText = source?.[language] || source?.en || '';

  const borderStyle = style === 'bordered' ? `border-left: 4px solid ${accent_color || '#50a5f1'}; padding-left: 20px;` : '';
  const bgStyle = style === 'filled' ? 'background-color: #f8f9fa; padding: 24px; border-radius: 8px;' : '';

  return `
    <figure class="blockquote blockquote--${style}" style="${borderStyle}${bgStyle}">
      <blockquote>${quoteText}</blockquote>
      ${authorText || sourceText ? `
        <figcaption>
          ${authorText ? `<strong>${authorText}</strong>` : ''}
          ${sourceText ? `<cite>— ${sourceText}</cite>` : ''}
        </figcaption>
      ` : ''}
    </figure>
  `;
}
```
