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
| `widget.quote` | object | Multilingual quote text |
| `widget.author` | object | Multilingual author name |
| `widget.source` | object | Multilingual source/publication |
| `widget.style` | string | Quote style: `"simple"`, `"bordered"`, `"filled"` |
| `widget.accent_color` | string\|null | Accent color for border/icon |
| `widget.hover_accent_color` | string\|null | Accent color on hover |
| `widget.hover_lift` | number | Hover lift in pixels — translateY offset (default: `0`) |
| `widget.hover_scale` | number | Hover scale factor (default: `1`) |
| `widget.hover_shadow` | string | Hover shadow preset: `"none"`, `"sm"`, `"md"`, `"lg"` (default: `"none"`) |
| `widget.transition_duration` | number | Hover transition duration in ms (default: 200) |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "blockquote",
  "uuid": "blockquote-123",
  "widget": {
    "quote": {
      "en": "The only way to do great work is to love what you do.",
      "pl": "Jedynym sposobem na wykonanie swietnej pracy jest kochanie tego, co sie robi."
    },
    "author": {
      "en": "Steve Jobs",
      "pl": "Steve Jobs"
    },
    "source": {
      "en": "Stanford Commencement Speech, 2005",
      "pl": "Przemowienie na Stanford, 2005"
    },
    "style": "bordered",
    "accent_color": "#50a5f1",
    "hover_accent_color": null,
    "hover_lift": 0,
    "hover_scale": 1,
    "hover_shadow": "none",
    "transition_duration": 200
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
  const { quote, author, source, style, accent_color } = widget.widget;

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
          ${sourceText ? `<cite>-- ${sourceText}</cite>` : ''}
        </figcaption>
      ` : ''}
    </figure>
  `;
}
```
