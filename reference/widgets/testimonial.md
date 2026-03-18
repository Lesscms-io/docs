# Testimonial Widget

A customer testimonial or review card with quote, author info, avatar and optional rating. Uses element-group governance — each visual element is a nested object.

## Widget Type

```
testimonial
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"testimonial"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget data (element groups) |
| `widget.quote` | object | Quote element group |
| `widget.quote.text` | object | Multilingual quote text |
| `widget.quote.color` | string\|null | Quote text color |
| `widget.quote.color:hover` | string\|null | Quote text color on hover |
| `widget.author` | object | Author element group |
| `widget.author.text` | object | Multilingual author name |
| `widget.author.color` | string\|null | Author text color |
| `widget.author.color:hover` | string\|null | Author text color on hover |
| `widget.position` | object | Position element group |
| `widget.position.text` | object | Multilingual position/title text |
| `widget.position.color` | string\|null | Position text color |
| `widget.position.color:hover` | string\|null | Position text color on hover |
| `widget.avatar` | object | Avatar element group |
| `widget.avatar.image` | string\|null | Avatar image URL |
| `widget.rating` | object | Rating element group |
| `widget.rating.value` | number | Star rating value 1-5 (default: 5) |
| `widget.rating.color` | string\|null | Star rating color |
| `widget.rating.color:hover` | string\|null | Star rating color on hover |
| `widget.config` | object | Configuration element group |
| `widget.config.alignment` | string | Text alignment: `"left"`, `"center"`, `"right"` (default: `"center"`) |
| `settings` | object | Layout settings (margin, width, height) |

## Example Response

```json
{
  "widget_type": "testimonial",
  "uuid": "testimonial-123",
  "widget": {
    "quote": {
      "text": { "pl": "Fantastyczna wspolpraca!", "en": "Working with this team has been an absolute pleasure." },
      "color": "var:dark",
      "color:hover": null
    },
    "author": {
      "text": { "pl": "Jan Kowalski", "en": "John Smith" },
      "color": "var:dark",
      "color:hover": null
    },
    "position": {
      "text": { "pl": "Dyrektor, Firma Tech", "en": "CEO, Tech Company" },
      "color": "var:muted",
      "color:hover": null
    },
    "avatar": {
      "image": "https://cdn.example.com/avatars/john.jpg"
    },
    "rating": {
      "value": 5,
      "color": null,
      "color:hover": null
    },
    "config": {
      "alignment": "center"
    }
  },
  "settings": {}
}
```

## Wrapper Support

Testimonial widgets support wrapping — multiple testimonials can be grouped in a grid via the wrapper mechanism. The wrapper is a separate node in the page structure, not part of the widget itself.

## Usage Example

```javascript
function renderTestimonial(widget, language) {
  const { quote, author, position, avatar, rating, config } = widget.widget;

  const quoteText = quote.text?.[language] || quote.text?.pl || '';
  const authorText = author.text?.[language] || author.text?.pl || '';
  const positionText = position.text?.[language] || position.text?.pl || '';
  const alignment = config.alignment || 'center';

  // Generate stars
  let starsHtml = '';
  if (rating.value > 0) {
    const starColor = resolveColor(rating.color);
    for (let i = 1; i <= 5; i++) {
      const cls = i <= rating.value ? 'fa-solid fa-star' : 'fa-regular fa-star';
      starsHtml += `<i class="${cls}" style="color: ${starColor || '#ffc107'}"></i>`;
    }
  }

  return `
    <div class="testimonial-widget" style="text-align: ${alignment}">
      ${starsHtml ? `<div class="testimonial-rating">${starsHtml}</div>` : ''}
      <blockquote class="testimonial-quote" style="color: ${resolveColor(quote.color) || 'inherit'}">
        "${quoteText}"
      </blockquote>
      <div class="testimonial-author">
        ${avatar.image ? `<img src="${avatar.image}" alt="${authorText}" class="testimonial-avatar">` : ''}
        <div class="testimonial-author-info">
          <strong style="color: ${resolveColor(author.color) || 'inherit'}">${authorText}</strong>
          ${positionText ? `<span style="color: ${resolveColor(position.color) || 'inherit'}">${positionText}</span>` : ''}
        </div>
      </div>
    </div>
  `;
}
```
