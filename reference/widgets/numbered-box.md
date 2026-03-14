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
| `widget.title_tag` | string | HTML heading tag for title (default: `"h3"`) |
| `widget.title_color` | string\|null | Title heading color (hex or `"var:..."`) |
| `widget.text_color` | string\|null | Text paragraph color (hex or `"var:..."`) |
| `widget.title_font_size` | string\|null | Title font size (e.g. `"24px"`, `"1.5rem"`) |
| `widget.title_font_weight` | string\|null | Title font weight (e.g. `"400"`, `"700"`) |
| `widget.text_font_size` | string\|null | Text font size (e.g. `"16px"`, `"1rem"`) |
| `widget.text_font_weight` | string\|null | Text font weight (e.g. `"400"`, `"700"`) |
| `widget.hover_card_background` | string\|null | Card background color on hover |
| `widget.hover_card_border_color` | string\|null | Card border color on hover |
| `widget.hover_number_color` | string\|null | Number color on hover |
| `widget.hover_number_background` | string\|null | Number background color on hover |
| `widget.hover_title_color` | string\|null | Title heading color on hover |
| `widget.hover_text_color` | string\|null | Text paragraph color on hover |
| `widget.hover_lift` | number | Hover lift in pixels — translateY offset (default: `0`) |
| `widget.hover_scale` | number | Hover scale factor (default: `1`) |
| `widget.hover_shadow` | string | Hover shadow preset: `"none"`, `"sm"`, `"md"`, `"lg"` (default: `"none"`) |
| `widget.transition_duration` | number | Hover transition duration in ms (default: 200) |
| `widget.title` | object | Multilingual plain text title (separate from rich text content) |
| `widget.html` | object | Multilingual HTML content (description/body text) |
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
    "title_tag": "h3",
    "title_color": null,
    "text_color": null,
    "title_font_size": null,
    "title_font_weight": null,
    "text_font_size": null,
    "text_font_weight": null,
    "hover_card_background": null,
    "hover_card_border_color": null,
    "hover_number_color": null,
    "hover_number_background": null,
    "hover_title_color": null,
    "hover_text_color": null,
    "hover_lift": 0,
    "hover_scale": 1,
    "hover_shadow": "none",
    "transition_duration": 200,
    "title": {
      "en": "Discovery",
      "pl": "Odkrywanie"
    },
    "html": {
      "en": "<p>We learn about your business needs and goals.</p>",
      "pl": "<p>Poznajemy potrzeby i cele Twojego biznesu.</p>"
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
