# Icon Box Widget

A widget combining an icon with rich text content, commonly used for feature highlights.

## Widget Type

```
icon-box
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"icon-box"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget data |
| `widget.content_source` | string | Content source: `"static"` or `"dynamic"` (default: `"static"`) |
| `widget.icon` | string | Icon class (e.g., `"bx bx-phone"`) |
| `widget.icon_size` | number | Icon size in pixels (default: 48) |
| `widget.icon_color` | string | Icon color (hex, default: `"#50a5f1"`) |
| `widget.icon_background` | string | Icon background color (default: `"transparent"`) |
| `widget.icon_padding` | number | Icon padding in pixels (default: 0) |
| `widget.icon_border_radius` | number | Icon border radius in pixels (default: 0) |
| `widget.icon_position` | string | `"left"`, `"right"`, `"top"`, `"bottom"` (default: `"left"`) |
| `widget.icon_vertical_align` | string | `"top"`, `"center"`, `"bottom"` (default: `"top"`) |
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
| `widget.icon` | string | Icon class |
| `widget.content_source` | string | `"static"` or `"dynamic"` |
| `widget.collection_code` | string | Collection code (when dynamic) |
| `widget.field_code` | string | Field code (when dynamic) |
| `widget.entry_id` | string | Entry ID (when dynamic) |
| `widget.entry_source` | string | Entry source: `"static"` or `"url"` |
| `widget.entry_url_segment` | number | URL segment index |
| `widget.html` | object | Multilingual HTML content |

## Example Response (Static)

```json
{
  "widget_type": "icon-box",
  "uuid": "iconbox-123",
  "widget": {
    "content_source": "static",
    "icon": "bx bx-rocket",
    "icon_size": 48,
    "icon_color": "#50a5f1",
    "icon_background": "transparent",
    "icon_padding": 0,
    "icon_border_radius": 0,
    "icon_position": "left",
    "icon_vertical_align": "top",
    "card_background": "",
    "card_padding": "",
    "card_border_radius": 0,
    "card_border_color": "",
    "html": {
      "en": "<h3>Fast Delivery</h3><p>We deliver your order within 24 hours.</p>",
      "pl": "<h3>Szybka dostawa</h3><p>Dostarczamy zamowienia w ciagu 24 godzin.</p>"
    }
  },
  "settings": {
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Example Response (Dynamic Content)

```json
{
  "widget_type": "icon-box",
  "uuid": "iconbox-456",
  "widget": {
    "content_source": "dynamic",
    "icon": "bx bx-news",
    "icon_size": 48,
    "icon_color": "#50a5f1",
    "icon_background": "transparent",
    "icon_padding": 0,
    "icon_border_radius": 0,
    "icon_position": "left",
    "icon_vertical_align": "top",
    "card_background": "",
    "card_padding": "",
    "card_border_radius": 0,
    "card_border_color": "",
    "collection_code": "services",
    "field_code": "description",
    "entry_id": "entry-123",
    "entry_source": "url",
    "entry_url_segment": 1
  },
  "settings": {}
}
```

## Icon Position

| Value | Description |
|-------|-------------|
| `left` | Icon on the left, content on the right |
| `right` | Icon on the right, content on the left |
| `top` | Icon above the content |
| `bottom` | Icon below the content |

## Icon Vertical Align

Used when `icon_position` is `"left"` or `"right"`:

| Value | Description |
|-------|-------------|
| `top` | Icon aligned to top of content |
| `center` | Icon centered vertically |
| `bottom` | Icon aligned to bottom of content |

## Icon Size

| Value | Description |
|-------|-------------|
| `"24"` | 24px icon |
| `"32"` | 32px icon |
| `"48"` | 48px icon |
| `"64"` | 64px icon |

## Multi-Item Support

The icon-box widget supports displaying multiple icon boxes in a grid. When multiple items are configured, the API returns a multi-item structure with `multi_item: true`, `multi_columns`, and `items[]` array. Each item has the same structure as a single icon-box widget (without `uuid` and `settings`). Shared style fields (icon_position, icon_size, icon_color, etc.) are the same across all items.

```json
{
  "widget_type": "icon-box",
  "uuid": "iconbox-multi",
  "multi_item": true,
  "multi_columns": 3,
  "multi_gap": 16,
  "items": [
    {
      "widget_type": "icon-box",
      "widget": { "icon": "bx bx-rocket", "icon_size": "48", "icon_color": "#50a5f1", "icon_position": "top", "html": { "en": "<h3>Fast</h3><p>Lightning fast delivery.</p>" } }
    },
    {
      "widget_type": "icon-box",
      "widget": { "icon": "bx bx-shield", "icon_size": "48", "icon_color": "#50a5f1", "icon_position": "top", "html": { "en": "<h3>Secure</h3><p>Enterprise-grade security.</p>" } }
    },
    {
      "widget_type": "icon-box",
      "widget": { "icon": "bx bx-support", "icon_size": "48", "icon_color": "#50a5f1", "icon_position": "top", "html": { "en": "<h3>Support</h3><p>24/7 customer support.</p>" } }
    }
  ],
  "settings": {}
}
```

## Usage Example

```javascript
function renderIconBox(widget, language) {
  const { icon, icon_size, icon_color, icon_background, icon_border_radius, icon_position, icon_vertical_align } = widget.widget;
  const html = widget.widget?.html?.[language] || widget.widget?.html?.en || '';

  const iconStyle = [
    `font-size: ${icon_size}px`,
    `color: ${icon_color}`,
    icon_background ? `background: ${icon_background}` : '',
    icon_border_radius ? `border-radius: ${icon_border_radius}px; padding: 12px;` : ''
  ].filter(Boolean).join('; ');

  const iconHtml = `<i class="${icon}" style="${iconStyle}"></i>`;
  const flexDir = icon_position === 'top' ? 'column' : icon_position === 'bottom' ? 'column-reverse' : icon_position === 'right' ? 'row-reverse' : 'row';
  const alignItems = icon_vertical_align === 'top' ? 'flex-start' : icon_vertical_align === 'bottom' ? 'flex-end' : 'center';

  return `
    <div class="icon-box" style="display: flex; flex-direction: ${flexDir}; align-items: ${alignItems}; gap: 16px;">
      <div class="icon-box__icon">${iconHtml}</div>
      <div class="icon-box__content">${html}</div>
    </div>
  `;
}
```
