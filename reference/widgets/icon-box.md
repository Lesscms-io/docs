# Icon Box Widget

A widget combining an icon with text content, commonly used for feature highlights.

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
| `config.content_source` | string | `"static"` or `"dynamic"` |
| `config.icon` | string | Icon class (e.g., `"bx bx-phone"`) |
| `config.icon_size` | number | Icon size in pixels |
| `config.icon_color` | string | Icon color (hex) |
| `config.icon_background` | string | Icon background color |
| `config.icon_padding` | number | Icon padding in pixels |
| `config.icon_border_radius` | number | Icon border radius in pixels |
| `config.icon_position` | string | `"left"`, `"right"`, `"top"` |
| `config.icon_vertical_align` | string | `"top"`, `"center"`, `"bottom"` |
| `config.collection_code` | string | Collection code (when dynamic) |
| `config.field_code` | string | Field code (when dynamic) |
| `config.entry_id` | string | Entry ID (when dynamic) |
| `content` | object | Widget content (when static) |
| `content.html` | object | Multilingual HTML content |
| `settings` | object | Style settings (optional) |

## Example Response (Static)

```json
{
  "widget_type": "icon-box",
  "uuid": "iconbox-123",
  "config": {
    "content_source": "static",
    "icon": "bx bx-rocket",
    "icon_size": 48,
    "icon_color": "#50a5f1",
    "icon_background": "transparent",
    "icon_padding": 0,
    "icon_border_radius": 0,
    "icon_position": "left",
    "icon_vertical_align": "top"
  },
  "content": {
    "html": {
      "en": "<h3>Fast Delivery</h3><p>We deliver your order within 24 hours.</p>",
      "pl": "<h3>Szybka dostawa</h3><p>Dostarczamy zamówienia w ciągu 24 godzin.</p>"
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

## Example Response (Dynamic)

```json
{
  "widget_type": "icon-box",
  "uuid": "iconbox-456",
  "config": {
    "content_source": "dynamic",
    "icon": "bx bx-check-circle",
    "icon_size": 32,
    "icon_color": "#28a745",
    "icon_background": "#e8f5e9",
    "icon_padding": 12,
    "icon_border_radius": 8,
    "icon_position": "top",
    "icon_vertical_align": "center",
    "collection_code": "features",
    "field_code": "description",
    "entry_id": "feature-1"
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
      "config": { "content_source": "static", "icon": "bx bx-rocket", "icon_size": 48, "icon_color": "#50a5f1", "icon_position": "top" },
      "content": { "html": { "en": "<h3>Fast</h3><p>Lightning fast delivery.</p>" } }
    },
    {
      "widget_type": "icon-box",
      "config": { "content_source": "static", "icon": "bx bx-shield", "icon_size": 48, "icon_color": "#50a5f1", "icon_position": "top" },
      "content": { "html": { "en": "<h3>Secure</h3><p>Enterprise-grade security.</p>" } }
    },
    {
      "widget_type": "icon-box",
      "config": { "content_source": "static", "icon": "bx bx-support", "icon_size": 48, "icon_color": "#50a5f1", "icon_position": "top" },
      "content": { "html": { "en": "<h3>Support</h3><p>24/7 customer support.</p>" } }
    }
  ],
  "settings": {}
}
```

## Usage Example

```javascript
function renderIconBox(widget, language) {
  const { icon, icon_size, icon_color, icon_position } = widget.config;
  const html = widget.content?.html?.[language] || widget.content?.html?.en || '';

  const iconHtml = `<i class="${icon}" style="font-size: ${icon_size}px; color: ${icon_color};"></i>`;
  const flexDir = icon_position === 'top' ? 'column' : 'row';

  return `
    <div class="icon-box" style="display: flex; flex-direction: ${flexDir}; gap: 16px;">
      <div class="icon-box__icon">${iconHtml}</div>
      <div class="icon-box__content">${html}</div>
    </div>
  `;
}
```
