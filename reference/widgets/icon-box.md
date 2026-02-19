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
| `config` | object | Widget configuration |
| `config.icon` | string | Icon class (e.g., `"bx bx-phone"`) |
| `config.icon_size` | string | Icon size: `"24"`, `"32"`, `"48"`, `"64"` (pixels) |
| `config.icon_color` | string | Icon color (hex) |
| `config.icon_background` | string | Icon background color (hex) |
| `config.icon_border_radius` | number | Icon border radius in pixels |
| `config.icon_position` | string | `"left"`, `"right"`, `"top"`, `"bottom"` |
| `config.icon_vertical_align` | string | `"top"`, `"center"`, `"bottom"` |
| `content` | object | Widget content |
| `content.html` | object | Multilingual HTML content |
| `settings` | object | Style settings (optional) |

### Per-Item Fields (Multi-Item)

When used in multi-item mode, each item has:

| Property | Type | Description |
|----------|------|-------------|
| `config.icon` | string | Icon class |
| `config.content_source` | string | `"static"` or `"dynamic"` |
| `config.collection_code` | string | Collection code (when dynamic) |
| `config.field_code` | string | Field code (when dynamic) |
| `config.entry_id` | string | Entry ID (when dynamic) |
| `config.entry_source` | string | Entry source: `"static"` or `"url"` |
| `config.entry_url_segment` | number | URL segment index |
| `content.html` | object | Multilingual HTML content |

## Example Response (Static)

```json
{
  "widget_type": "icon-box",
  "uuid": "iconbox-123",
  "config": {
    "icon": "bx bx-rocket",
    "icon_size": "48",
    "icon_color": "#50a5f1",
    "icon_background": "transparent",
    "icon_border_radius": 0,
    "icon_position": "left",
    "icon_vertical_align": "top"
  },
  "content": {
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
      "config": { "icon": "bx bx-rocket", "icon_size": "48", "icon_color": "#50a5f1", "icon_position": "top" },
      "content": { "html": { "en": "<h3>Fast</h3><p>Lightning fast delivery.</p>" } }
    },
    {
      "widget_type": "icon-box",
      "config": { "icon": "bx bx-shield", "icon_size": "48", "icon_color": "#50a5f1", "icon_position": "top" },
      "content": { "html": { "en": "<h3>Secure</h3><p>Enterprise-grade security.</p>" } }
    },
    {
      "widget_type": "icon-box",
      "config": { "icon": "bx bx-support", "icon_size": "48", "icon_color": "#50a5f1", "icon_position": "top" },
      "content": { "html": { "en": "<h3>Support</h3><p>24/7 customer support.</p>" } }
    }
  ],
  "settings": {}
}
```

## Usage Example

```javascript
function renderIconBox(widget, language) {
  const { icon, icon_size, icon_color, icon_background, icon_border_radius, icon_position, icon_vertical_align } = widget.config;
  const html = widget.content?.html?.[language] || widget.content?.html?.en || '';

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
