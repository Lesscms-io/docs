# Numbered Box Widget

A widget combining an auto-generated number (01, 02, 03...) with a heading and rich text content. Uses element-group governance — each visual element is a nested object. Commonly used for step-by-step processes, feature lists, or numbered highlights.

## Widget Type

```
numbered-box
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"numbered-box"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget data (element groups) |
| `widget.number` | object | Number element group |
| `widget.number.color` | string\|null | Number text color |
| `widget.number.color:hover` | string\|null | Number text color on hover |
| `widget.number.background` | string\|null | Number background color |
| `widget.number.background:hover` | string\|null | Number background color on hover |
| `widget.number.size` | number | Number font size in pixels (default: 48) |
| `widget.number.font_weight` | number | Number font weight: 400, 600, 700, 900 (default: 700) |
| `widget.number.padding` | number | Number padding in pixels (default: 12) |
| `widget.number.border_radius` | number | Number border radius in pixels (default: 8) |
| `widget.number.position` | string | Number position: `"left"`, `"right"`, `"top"`, `"bottom"` (default: `"left"`) |
| `widget.number.vertical_align` | string | Number vertical alignment: `"top"`, `"center"`, `"bottom"` (default: `"top"`) |
| `widget.heading` | object | Heading element group |
| `widget.heading.html` | object | Multilingual heading text |
| `widget.heading.color` | string\|null | Heading text color |
| `widget.heading.color:hover` | string\|null | Heading text color on hover |
| `widget.heading.tag` | string | HTML heading tag (default: `"h3"`) |
| `widget.text` | object | Text element group |
| `widget.text.html` | object | Multilingual HTML content |
| `widget.text.color` | string\|null | Content text color |
| `widget.text.color:hover` | string\|null | Content text color on hover |
| `settings` | object | Style settings (shared widget container styles) |

## Example Response

```json
{
  "widget_type": "numbered-box",
  "uuid": "numbox-123",
  "widget": {
    "number": {
      "color": "var:primary",
      "color:hover": null,
      "background": "var:primary:15",
      "background:hover": null,
      "size": 48,
      "font_weight": 700,
      "padding": 12,
      "border_radius": 8,
      "position": "left",
      "vertical_align": "top"
    },
    "heading": {
      "html": { "en": "Discovery", "pl": "Odkrywanie" },
      "color": null,
      "color:hover": null,
      "tag": "h3"
    },
    "text": {
      "html": { "en": "<p>We learn about your business needs and goals.</p>", "pl": "<p>Poznajemy potrzeby i cele Twojego biznesu.</p>" },
      "color": null,
      "color:hover": null
    }
  },
  "settings": {}
}
```

## Number Position

| Value | Description |
|-------|-------------|
| `left` | Number on the left, content on the right |
| `right` | Number on the right, content on the left |
| `top` | Number above the content |
| `bottom` | Number below the content |

## Number Vertical Align

Used when `number.position` is `"left"` or `"right"`:

| Value | Description |
|-------|-------------|
| `top` | Number aligned to top of content |
| `center` | Number centered vertically |
| `bottom` | Number aligned to bottom of content |

## Usage Example

```javascript
function renderNumberedBox(widget, language, itemIndex = 0) {
  const { number, heading, text } = widget.widget;
  const displayNumber = String(itemIndex + 1).padStart(2, '0');

  const numberStyle = [
    `font-size: ${number.size}px`,
    `color: ${number.color}`,
    `font-weight: ${number.font_weight}`,
    number.background ? `background: ${number.background}` : '',
    number.padding ? `padding: ${number.padding}px` : '',
    number.border_radius ? `border-radius: ${number.border_radius}px` : ''
  ].filter(Boolean).join('; ');

  const flexDir = number.position === 'top' ? 'column' : number.position === 'bottom' ? 'column-reverse' : number.position === 'right' ? 'row-reverse' : 'row';
  const alignItems = number.vertical_align === 'top' ? 'flex-start' : number.vertical_align === 'bottom' ? 'flex-end' : 'center';

  const titleTag = heading.tag || 'h3';
  const titleText = heading.html?.[language] || heading.html?.en || '';
  const bodyHtml = text.html?.[language] || text.html?.en || '';

  return `
    <div class="numbered-box" style="display: flex; flex-direction: ${flexDir}; align-items: ${alignItems}; gap: 16px;">
      <div class="numbered-box__number" style="${numberStyle}; line-height: 1; font-variant-numeric: tabular-nums;">${displayNumber}</div>
      <div class="numbered-box__content">
        <${titleTag}>${titleText}</${titleTag}>
        ${bodyHtml}
      </div>
    </div>
  `;
}
```
