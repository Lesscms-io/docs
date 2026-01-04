# Collection Field Widget

Display a single field from a collection entry with customizable rendering and styling.

## Widget Type

```
collection-field
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"collection-field"` |
| `uuid` | string | Unique widget identifier |
| `config` | object | Widget configuration |
| `config.collection_code` | string\|null | Collection code |
| `config.field_code` | string\|null | Field code to display |
| `config.field_type` | string | Field type (default: `"text"`) |
| `config.display_as` | string | Render element: `"p"`, `"h1"`-`"h6"`, `"span"` (default: `"p"`) |
| `config.entry_source` | string | Entry source: `"static"` or `"url"` (default: `"static"`) |
| `config.entry_id` | string\|null | Specific entry ID (when `entry_source` is `"static"`) |
| `config.entry_url_segment` | number | URL segment index (when `entry_source` is `"url"`, default: 1) |
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
| `config.link_text` | object | Multilingual link button text (for _link field) |
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

## Example Response (Static Entry)

```json
{
  "widget_type": "collection-field",
  "uuid": "field-123",
  "config": {
    "collection_code": "team",
    "field_code": "bio",
    "field_type": "richtext",
    "display_as": "p",
    "entry_source": "static",
    "entry_id": "entry-uuid-456",
    "entry_url_segment": 1,
    "label": { "en": "Biography", "pl": "Biografia" },
    "label_position": "above",
    "label_background": null,
    "label_color": "#333333",
    "label_padding": 0,
    "label_font_size": "14px",
    "label_font_weight": "bold",
    "value_background": null,
    "value_color": null,
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
  "settings": {
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Example Response (URL-based Entry)

```json
{
  "widget_type": "collection-field",
  "uuid": "field-456",
  "config": {
    "collection_code": "products",
    "field_code": "description",
    "field_type": "text",
    "display_as": "p",
    "entry_source": "url",
    "entry_id": null,
    "entry_url_segment": 2,
    "label": {},
    "label_position": "hidden",
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

## Entry Source Values

| Value | Description |
|-------|-------------|
| `static` | Use specific `entry_id` |
| `url` | Extract entry ID from URL segment at `entry_url_segment` index |

## Label Position Values

| Value | Description |
|-------|-------------|
| `hidden` | Don't show label |
| `above` | Label above the value |
| `inline` | Label inline with value |

## Display As Values

| Value | Description |
|-------|-------------|
| `p` | Paragraph |
| `h1` - `h6` | Heading levels |
| `span` | Inline span |

## Date Format Values

| Value | Description |
|-------|-------------|
| `full` | Full date (e.g., "January 15, 2024") |
| `short` | Short date (e.g., "01/15/24") |
| `relative` | Relative time (e.g., "2 days ago") |
| `custom` | Use `custom_date_format` string |

## Usage Example

```javascript
async function renderCollectionField(widget, language, urlSegments, api) {
  const {
    collection_code, field_code, display_as,
    entry_source, entry_id, entry_url_segment,
    label, label_position,
    show_icon, icon, icon_position
  } = widget.config;

  // Determine entry ID
  let targetEntryId = entry_id;
  if (entry_source === 'url' && entry_url_segment) {
    targetEntryId = urlSegments[entry_url_segment - 1];
  }

  if (!targetEntryId || !collection_code || !field_code) {
    return '';
  }

  // Fetch entry
  const entry = await api.getEntry(collection_code, targetEntryId);
  if (!entry) return '';

  // Get field value
  const fieldValue = entry.data[field_code];
  let content = '';

  // Handle multilingual fields
  if (typeof fieldValue === 'object' && fieldValue !== null && !Array.isArray(fieldValue)) {
    content = fieldValue[language] || fieldValue.en || '';
  } else {
    content = fieldValue || '';
  }

  // Build label HTML
  const labelHtml = label_position !== 'hidden' && label?.[language]
    ? `<span class="field-label">${label[language]}</span>`
    : '';

  // Build icon HTML
  const iconHtml = show_icon && icon
    ? `<i class="${icon}" style="font-size: ${widget.config.icon_size}px; color: ${widget.config.icon_color}"></i>`
    : '';

  // Render based on display_as
  const tag = display_as || 'p';
  const valueHtml = `<${tag} class="field-value">${content}</${tag}>`;

  return `
    <div class="collection-field ${label_position === 'inline' ? 'inline' : ''}">
      ${icon_position === 'left' ? iconHtml : ''}
      ${labelHtml}
      ${valueHtml}
      ${icon_position === 'right' ? iconHtml : ''}
    </div>
  `;
}
```

## Use Cases

- **Dynamic page titles**: Display entry title as h1
- **Product descriptions**: Show product description from collection
- **Entry metadata**: Display creation date, author, category
- **Price displays**: Show product prices with custom formatting
- **Contact info**: Display phone, email from entries
