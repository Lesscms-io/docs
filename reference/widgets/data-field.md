# Data Field Widget

Display a single field value with support for both static content and dynamic collection data.

## Widget Type

```
data-field
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"data-field"` |
| `uuid` | string | Unique widget identifier |
| `config` | object | Widget configuration |
| `config.value_source` | string | Value source: `"static"` or `"dynamic"` (default: `"dynamic"`) |
| `config.display_as` | string | HTML element: `"p"`, `"h1"`-`"h6"`, `"span"`, `"image"`, `"gallery"` (default: `"p"`) |
| `config.label` | object | Multilingual label text `{ "en": "...", "pl": "..." }` |
| `config.label_position` | string | Label position: `"hidden"`, `"above"`, `"inline"` (default: `"hidden"`) |
| `config.label_background` | string\|null | Label background color |
| `config.label_color` | string\|null | Label text color |
| `config.label_padding` | number | Label padding in px (default: 0) |
| `config.label_font_size` | string\|null | Label font size (e.g., `"14px"`) |
| `config.label_font_weight` | string\|null | Label font weight (e.g., `"bold"`) |
| `config.value_background` | string\|null | Value background color |
| `config.value_color` | string\|null | Value text color |
| `config.value_padding` | number | Value padding in px (default: 0) |
| `config.date_format` | string | Date format: `"full"`, `"short"`, `"relative"`, `"custom"` (default: `"full"`) |
| `config.show_time` | boolean | Show time for datetime fields (default: true) |
| `config.custom_date_format` | string\|null | Custom date format string |
| `config.link_text` | object | Multilingual link button text |
| `config.button_style` | string | Button style: `"primary"`, `"secondary"`, `"outline"` (default: `"primary"`) |
| `config.button_size` | string | Button size: `"sm"`, `"md"`, `"lg"` (default: `"md"`) |
| `config.show_icon` | boolean | Show icon (default: false) |
| `config.icon` | string\|null | Icon class (e.g., `"bx bx-link"`) |
| `config.icon_position` | string | Icon position: `"left"`, `"right"` (default: `"left"`) |
| `config.icon_size` | string | Icon size in px (default: `"24"`) |
| `config.icon_color` | string | Icon color (default: `"#50a5f1"`) |
| `config.icon_background` | string | Icon background (default: `"transparent"`) |
| `config.icon_padding` | string | Icon padding (default: `"0"`) |
| `config.icon_border_radius` | string | Icon border radius (default: `"0"`) |
| `config.icon_gap` | string | Gap between icon and content (default: `"12"`) |
| `settings` | object | Style settings (optional) |

### Dynamic Mode Additional Fields

When `value_source` is `"dynamic"`:

| Property | Type | Description |
|----------|------|-------------|
| `config.collection_code` | string\|null | Collection code |
| `config.field_code` | string\|null | Field code to display |
| `config.field_type` | string | Field type (default: `"text"`) |
| `config.entry_source` | string | Entry source: `"static"` or `"url"` (default: `"static"`) |
| `config.entry_id` | string\|null | Specific entry ID (when `entry_source` is `"static"`) |
| `config.entry_url_segment` | number | URL segment index (when `entry_source` is `"url"`, default: 1) |

### Static Mode Additional Fields

When `value_source` is `"static"`:

| Property | Type | Description |
|----------|------|-------------|
| `content.static_value` | object | Multilingual static value `{ "en": "...", "pl": "..." }` |

## Example Response (Dynamic Mode)

```json
{
  "widget_type": "data-field",
  "uuid": "field-123",
  "config": {
    "value_source": "dynamic",
    "display_as": "h2",
    "collection_code": "team",
    "field_code": "name",
    "field_type": "text",
    "entry_source": "url",
    "entry_id": null,
    "entry_url_segment": 2,
    "label": { "en": "Name", "pl": "Imie" },
    "label_position": "above",
    "label_background": null,
    "label_color": "#666666",
    "label_padding": 0,
    "label_font_size": "12px",
    "label_font_weight": "600",
    "value_background": null,
    "value_color": "#333333",
    "value_padding": 0,
    "date_format": "full",
    "show_time": true,
    "custom_date_format": null,
    "link_text": {},
    "button_style": "primary",
    "button_size": "md",
    "show_icon": false,
    "icon": null,
    "icon_position": "left",
    "icon_size": "24",
    "icon_color": "#50a5f1",
    "icon_background": "transparent",
    "icon_padding": "0",
    "icon_border_radius": "0",
    "icon_gap": "12"
  },
  "settings": {}
}
```

## Example Response (Static Mode)

```json
{
  "widget_type": "data-field",
  "uuid": "field-456",
  "config": {
    "value_source": "static",
    "display_as": "p",
    "label": { "en": "Note", "pl": "Uwaga" },
    "label_position": "inline",
    "label_background": null,
    "label_color": null,
    "label_padding": 0,
    "label_font_size": null,
    "label_font_weight": null,
    "value_background": null,
    "value_color": null,
    "value_padding": 0,
    "date_format": "full",
    "show_time": true,
    "custom_date_format": null,
    "link_text": {},
    "button_style": "primary",
    "button_size": "md",
    "show_icon": true,
    "icon": "bx bx-info-circle",
    "icon_position": "left",
    "icon_size": "20",
    "icon_color": "#50a5f1",
    "icon_background": "transparent",
    "icon_padding": "0",
    "icon_border_radius": "0",
    "icon_gap": "8"
  },
  "content": {
    "static_value": {
      "en": "This is a static value",
      "pl": "To jest statyczna wartość"
    }
  },
  "settings": {}
}
```

## Value Source

| Value | Description |
|-------|-------------|
| `static` | Use static multilingual value from `content.static_value` |
| `dynamic` | Fetch value from collection entry |

## Entry Source (Dynamic Mode)

| Value | Description |
|-------|-------------|
| `static` | Use specific `entry_id` |
| `url` | Extract entry ID from URL segment at `entry_url_segment` index |

## Display As Values

| Value | Description |
|-------|-------------|
| `p` | Paragraph |
| `h1` - `h6` | Heading levels |
| `span` | Inline span |
| `image` | Render as image |
| `gallery` | Render as gallery |

## Label Position Values

| Value | Description |
|-------|-------------|
| `hidden` | Don't show label |
| `above` | Label above the value |
| `inline` | Label inline with value |

## Date Format Values

| Value | Description |
|-------|-------------|
| `full` | Full date (e.g., "January 15, 2024") |
| `short` | Short date (e.g., "01/15/24") |
| `relative` | Relative time (e.g., "2 days ago") |
| `custom` | Use `custom_date_format` string |

## Usage Example

```javascript
async function renderDataField(widget, language, urlSegments, api) {
  const { value_source, display_as, show_icon, icon, icon_position } = widget.config;

  let content = '';

  if (value_source === 'static') {
    // Static mode - get value directly
    const staticValue = widget.content?.static_value || {};
    content = staticValue[language] || staticValue.en || '';
  } else {
    // Dynamic mode - fetch from collection
    const { collection_code, field_code, entry_source, entry_id, entry_url_segment } = widget.config;

    let targetEntryId = entry_id;
    if (entry_source === 'url' && entry_url_segment) {
      targetEntryId = urlSegments[entry_url_segment - 1];
    }

    if (targetEntryId && collection_code && field_code) {
      const entry = await api.getEntry(collection_code, targetEntryId);
      if (entry) {
        const fieldValue = entry.data[field_code];
        if (typeof fieldValue === 'object' && !Array.isArray(fieldValue)) {
          content = fieldValue[language] || fieldValue.en || '';
        } else {
          content = fieldValue || '';
        }
      }
    }
  }

  // Build icon HTML
  const iconHtml = show_icon && icon
    ? `<i class="${icon}" style="font-size: ${widget.config.icon_size}px; color: ${widget.config.icon_color}"></i>`
    : '';

  // Render based on display_as
  const tag = display_as || 'p';

  return `
    <div class="data-field">
      ${icon_position === 'left' ? iconHtml : ''}
      <${tag} class="data-field-value">${content}</${tag}>
      ${icon_position === 'right' ? iconHtml : ''}
    </div>
  `;
}
```

## Use Cases

- **Static labels**: Display fixed text with styling
- **Dynamic content**: Show collection field values
- **URL-based entries**: Display data based on URL segments
- **Formatted dates**: Show dates with custom formatting
- **Labeled values**: Key-value style displays with labels
