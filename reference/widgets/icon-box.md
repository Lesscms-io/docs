# Icon Box Widget

A content block with an icon, supporting various layouts and styling options.

## Widget Type

```
icon-box
```

## Data Properties

| Property | Type | Global | Description |
|----------|------|--------|-------------|
| `icon` | string | Yes | Font Awesome icon class |
| `content` | object | No | Rich text content (multilingual, HTML) |
| `icon_position` | string | Yes | Icon position: `left`, `right`, `top`, `bottom` |
| `icon_vertical_align` | string | Yes | Vertical alignment: `top`, `center`, `bottom` |
| `icon_size` | string | Yes | Icon size: `24`, `32`, `48`, `64` (pixels) |
| `icon_color` | string | Yes | Icon color (hex code) |
| `icon_background` | string | Yes | Icon background color (hex code) |

## Example Response

```json
{
  "widget_type": "icon-box",
  "uuid": "iconbox-123",
  "data": {
    "icon": "fa-solid fa-rocket",
    "content": {
      "en": "<h3>Fast Delivery</h3><p>We deliver your order within 24 hours.</p>",
      "pl": "<h3>Szybka dostawa</h3><p>Dostarczamy zamówienie w ciągu 24 godzin.</p>"
    },
    "icon_position": "left",
    "icon_vertical_align": "top",
    "icon_size": "48",
    "icon_color": "#FFFFFF",
    "icon_background": "#007BFF"
  },
  "settings": {
    "paddingTop": 20,
    "paddingBottom": 20
  }
}
```

## Icon Position Values

| Value | Description |
|-------|-------------|
| `left` | Icon on the left, content on the right |
| `right` | Icon on the right, content on the left |
| `top` | Icon above the content |
| `bottom` | Icon below the content |

## Vertical Alignment (for left/right positions)

| Value | Description |
|-------|-------------|
| `top` | Icon aligned to top of content |
| `center` | Icon centered vertically |
| `bottom` | Icon aligned to bottom of content |

## Usage Example

```javascript
// Render icon box widget
function renderIconBox(widget, language) {
  const {
    icon, content, icon_position, icon_size,
    icon_color, icon_background
  } = widget.data;

  const iconHtml = `
    <div class="icon-box-icon" style="
      font-size: ${icon_size}px;
      color: ${icon_color};
      background: ${icon_background};
      padding: 16px;
      border-radius: 50%;
    ">
      <i class="${icon}"></i>
    </div>
  `;

  const contentHtml = content?.[language] || content?.en || '';

  const isVertical = ['top', 'bottom'].includes(icon_position);

  return `
    <div class="icon-box" style="
      display: flex;
      flex-direction: ${isVertical ? 'column' : 'row'};
      gap: 16px;
    ">
      ${icon_position === 'right' || icon_position === 'bottom'
        ? contentHtml + iconHtml
        : iconHtml + contentHtml}
    </div>
  `;
}
```
