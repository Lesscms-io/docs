# Numbered Box Widget

A widget combining an auto-generated number (01, 02, 03...) with rich text content. Similar to Icon Box but uses sequential numbers instead of icons. Commonly used for step-by-step processes, feature lists, or numbered highlights.

## Widget Type

```
numbered-box
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"numbered-box"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget data |
| `widget.number_position` | string | `"left"`, `"right"`, `"top"`, `"bottom"` |
| `widget.number_vertical_align` | string | `"top"`, `"center"`, `"bottom"` |
| `widget.number_size` | number | Number font size in pixels (default: 48) |
| `widget.number_color` | string | Number color (hex, default: `"#00513b"`) |
| `widget.number_font_weight` | string | Font weight: `"400"`, `"600"`, `"700"`, `"900"` |
| `widget.number_background` | string | Number background color (hex or `"transparent"`) |
| `widget.number_padding` | number | Number padding in pixels (default: 0) |
| `widget.number_border_radius` | number | Number border radius in pixels (default: 0) |
| `widget.card_background` | string | Card background color (hex or `"var:..."`, default: `""`) |
| `widget.card_padding` | string | Card padding CSS value (e.g. `"20px"`, `"16px 24px"`, default: `""`) |
| `widget.card_border_radius` | number | Card border radius in pixels (default: 0) |
| `widget.card_border_color` | string | Card border color (hex or `"var:..."`, default: `""`) |
| `widget.html` | object | Multilingual HTML content |
| `settings` | object | Style settings (optional) |

### Per-Item Fields (Multi-Item)

When used in multi-item mode, each item has:

| Property | Type | Description |
|----------|------|-------------|
| `widget.html` | object | Multilingual HTML content |

Numbers are auto-generated from the item's position in the array (index 0 → "01", index 1 → "02", etc.). Config fields (number_position, number_size, etc.) are shared across all items.

## Example Response (Single)

```json
{
  "widget_type": "numbered-box",
  "uuid": "numbox-123",
  "widget": {
    "number_position": "left",
    "number_vertical_align": "top",
    "number_size": 48,
    "number_color": "#00513b",
    "number_font_weight": "700",
    "number_background": "transparent",
    "number_padding": 0,
    "number_border_radius": 0,
    "card_background": "",
    "card_padding": "",
    "card_border_radius": 0,
    "card_border_color": "",
    "html": {
      "en": "<h3>Discovery</h3><p>We learn about your business needs and goals.</p>",
      "pl": "<h3>Odkrywanie</h3><p>Poznajemy potrzeby i cele Twojego biznesu.</p>"
    }
  }
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

Used when `number_position` is `"left"` or `"right"`:

| Value | Description |
|-------|-------------|
| `top` | Number aligned to top of content |
| `center` | Number centered vertically |
| `bottom` | Number aligned to bottom of content |

## Multi-Item Support

The numbered-box widget supports displaying multiple numbered boxes in a grid. Numbers are automatically assigned based on item order: item 0 → "01", item 1 → "02", etc.

```json
{
  "widget_type": "numbered-box",
  "uuid": "numbox-multi",
  "multi_item": true,
  "multi_columns": 3,
  "multi_gap": 16,
  "items": [
    {
      "widget_type": "numbered-box",
      "widget": { "number_position": "left", "number_size": 48, "number_color": "#00513b", "number_font_weight": "700", "html": { "en": "<h3>Discovery</h3><p>We learn about your needs.</p>" } }
    },
    {
      "widget_type": "numbered-box",
      "widget": { "number_position": "left", "number_size": 48, "number_color": "#00513b", "number_font_weight": "700", "html": { "en": "<h3>Design</h3><p>We create the perfect solution.</p>" } }
    },
    {
      "widget_type": "numbered-box",
      "widget": { "number_position": "left", "number_size": 48, "number_color": "#00513b", "number_font_weight": "700", "html": { "en": "<h3>Deliver</h3><p>We launch and support your project.</p>" } }
    }
  ],
  "settings": {}
}
```

## Usage Example

```javascript
function renderNumberedBox(widget, language, itemIndex = 0) {
  const { number_position, number_vertical_align, number_size, number_color, number_font_weight, number_background, number_padding, number_border_radius } = widget.widget;
  const html = widget.widget?.html?.[language] || widget.widget?.html?.en || '';
  const number = String(itemIndex + 1).padStart(2, '0');

  const numberStyle = [
    `font-size: ${number_size}px`,
    `color: ${number_color}`,
    `font-weight: ${number_font_weight}`,
    number_background && number_background !== 'transparent' ? `background: ${number_background}` : '',
    number_padding ? `padding: ${number_padding}px` : '',
    number_border_radius ? `border-radius: ${number_border_radius}px` : ''
  ].filter(Boolean).join('; ');

  const flexDir = number_position === 'top' ? 'column' : number_position === 'bottom' ? 'column-reverse' : number_position === 'right' ? 'row-reverse' : 'row';
  const alignItems = number_vertical_align === 'top' ? 'flex-start' : number_vertical_align === 'bottom' ? 'flex-end' : 'center';

  return `
    <div class="numbered-box" style="display: flex; flex-direction: ${flexDir}; align-items: ${alignItems}; gap: 16px;">
      <div class="numbered-box__number" style="${numberStyle}; line-height: 1; font-variant-numeric: tabular-nums;">${number}</div>
      <div class="numbered-box__content">${html}</div>
    </div>
  `;
}
```
