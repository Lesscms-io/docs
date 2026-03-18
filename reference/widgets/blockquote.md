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
| `widget` | object | Widget properties |
| `widget.quote` | object | Quote element group |
| `widget.quote.text` | object | Multilingual quote text |
| `widget.quote.color` | string\|null | Accent color for border/icon |
| `widget.quote.color:hover` | string\|null | Accent color on hover |
| `widget.author` | object | Author element group |
| `widget.author.text` | object | Multilingual author name |
| `widget.source` | object | Source element group |
| `widget.source.text` | object | Multilingual source/publication |
| `widget.config` | object | Configuration group |
| `widget.config.blockquote_style` | string | Quote style: `"simple"`, `"bordered"`, `"filled"` |
| `settings` | object | Style settings (padding, background, border, hover transforms) |

## Example Response

```json
{
  "widget_type": "blockquote",
  "uuid": "blockquote-123",
  "widget": {
    "quote": {
      "text": {
        "en": "The only way to do great work is to love what you do.",
        "pl": "Jedynym sposobem na wykonanie swietnej pracy jest kochanie tego, co sie robi."
      },
      "color": "var:primary",
      "color:hover": null
    },
    "author": {
      "text": {
        "en": "Steve Jobs",
        "pl": "Steve Jobs"
      }
    },
    "source": {
      "text": {
        "en": "Stanford Commencement Speech, 2005",
        "pl": "Przemowienie na Stanford, 2005"
      }
    },
    "config": {
      "blockquote_style": "bordered"
    }
  },
  "settings": {
    "padding_top": 20,
    "padding_bottom": 20,
    "padding_left": 20,
    "padding_right": 20,
    "background_color": "var:background-alt",
    "border_radius": 8
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
  const { quote, author, source, config } = widget.widget;
  const style = config?.blockquote_style || 'bordered';

  const quoteText = quote?.text?.[language] || quote?.text?.en || '';
  const authorText = author?.text?.[language] || author?.text?.en || '';
  const sourceText = source?.text?.[language] || source?.text?.en || '';

  const accentColor = quote?.color || '#50a5f1';
  const borderStyle = style === 'bordered' ? `border-left: 4px solid ${accentColor}; padding-left: 20px;` : '';
  const bgStyle = style === 'filled' ? 'background-color: #f8f9fa; padding: 24px; border-radius: 8px;' : '';

  return `
    <figure class="blockquote blockquote--${style}" style="${borderStyle}${bgStyle}">
      <blockquote>${quoteText}</blockquote>
      ${authorText || sourceText ? `
        <figcaption>
          ${authorText ? `<strong>${authorText}</strong>` : ''}
          ${sourceText ? `<cite>-- ${sourceText}</cite>` : ''}
        </figcaption>
      ` : ''}
    </figure>
  `;
}
```
