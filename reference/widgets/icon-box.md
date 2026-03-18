# Icon Box Widget

A card with an icon and text content. Uses nested element-group structure. Wrappable for multi-column layouts.

## Widget Type

```
icon-box
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"icon-box"` |
| `uuid` | string | Unique widget identifier |
| `widget.icon` | object | Icon element |
| `widget.icon.icon` | string\|null | Icon class (e.g. `fa-solid fa-star`) |
| `widget.icon.size` | number | Icon size (px) |
| `widget.icon.padding` | number | Icon padding (px) |
| `widget.icon.border_radius` | number | Icon container border radius (px) |
| `widget.icon.color` | string\|null | Icon color |
| `widget.icon.color:hover` | string\|null | Icon color on hover |
| `widget.icon.background` | string\|null | Icon background color |
| `widget.icon.background:hover` | string\|null | Icon background on hover |
| `widget.icon.position` | string | Icon position: `left`, `right`, `top`, `bottom` |
| `widget.icon.vertical_align` | string | Vertical alignment: `top`, `center`, `bottom` |
| `widget.text` | object | Text element |
| `widget.text.content` | object | Multilingual text content (HTML) |
| `widget.text.color` | string\|null | Text color |
| `widget.text.color:hover` | string\|null | Text color on hover |
| `widget.text.tag` | string | HTML tag for content (p, div, span) |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Wrappable

Yes — can be wrapped in a multi-column grid layout.

## Example Response

```json
{
  "widget_type": "icon-box",
  "uuid": "ib-001",
  "widget": {
    "icon": {
      "icon": "fa-solid fa-star",
      "size": 32,
      "padding": 12,
      "border_radius": 8,
      "color": "var:primary",
      "color:hover": null,
      "background": "var:primary:15",
      "background:hover": null,
      "position": "top",
      "vertical_align": "top"
    },
    "text": {
      "content": {
        "en": "<p>Description of the icon box element.</p>",
        "pl": "<p>Opis elementu z ikoną.</p>"
      },
      "color": null,
      "color:hover": null,
      "tag": "p"
    }
  },
  "settings": {
    "padding_top": 20,
    "padding_right": 20,
    "padding_bottom": 20,
    "padding_left": 20,
    "border_radius": 8,
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Usage Example

```javascript
function renderIconBox(widget, language) {
  const { icon, text } = widget.widget;
  const tag = text.tag || 'p';
  const content = text.content?.[language] || text.content?.en || '';
  const isHorizontal = icon.position === 'left' || icon.position === 'right';
  const flexDir = icon.position === 'right' ? 'row-reverse'
    : icon.position === 'bottom' ? 'column-reverse'
    : isHorizontal ? 'row' : 'column';

  return `
    <div class="icon-box" style="display: flex; flex-direction: ${flexDir};
      align-items: ${isHorizontal ? (icon.vertical_align === 'center' ? 'center' : icon.vertical_align === 'bottom' ? 'flex-end' : 'flex-start') : 'center'};
      gap: 16px;">
      <div class="icon-box__icon" style="
        font-size: ${icon.size}px;
        padding: ${icon.padding}px;
        border-radius: ${icon.border_radius}px;
        color: ${icon.color || 'inherit'};
        background: ${icon.background || 'transparent'};">
        <i class="${icon.icon}"></i>
      </div>
      <div class="icon-box__content" style="color: ${text.color || 'inherit'}">
        <${tag}>${content}</${tag}>
      </div>
    </div>
  `;
}
```
