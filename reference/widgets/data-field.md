# Data Field Widget

Display a single field value from a collection entry with support for icons, labels, and various display formats. Uses nested element-group structure.

## Widget Type

```
data-field
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"data-field"` |
| `uuid` | string | Unique widget identifier |
| `widget.icon` | object | Icon element group |
| `widget.icon.show` | boolean | Whether to show icon (default: `false`) |
| `widget.icon.icon` | string\|null | Icon class (e.g. `"fa-solid fa-star"`) |
| `widget.icon.position` | string | Icon position: `"left"`, `"right"` (default: `"left"`) |
| `widget.icon.size` | number | Icon size in px (default: `24`) |
| `widget.icon.color` | string\|null | Icon color |
| `widget.icon.color:hover` | string\|null | Icon color on hover |
| `widget.icon.background` | string\|null | Icon background color |
| `widget.icon.background:hover` | string\|null | Icon background on hover |
| `widget.icon.padding` | number | Icon padding in px (default: `0`) |
| `widget.icon.border_radius` | number | Icon border radius in px (default: `0`) |
| `widget.icon.gap` | number | Gap between icon and content in px (default: `12`) |
| `widget.label` | object | Label element group |
| `widget.label.position` | string | Label position: `"hidden"`, `"above"`, `"inline"` (default: `"hidden"`) |
| `widget.label.html` | object | Multilingual label text `{ "en": "...", "pl": "..." }` |
| `widget.label.background` | string\|null | Label background color |
| `widget.label.background:hover` | string\|null | Label background on hover |
| `widget.label.color` | string\|null | Label text color |
| `widget.label.color:hover` | string\|null | Label text color on hover |
| `widget.label.padding` | number | Label padding in px (default: `0`) |
| `widget.label.font_size` | string\|null | Label font size (e.g. `"14px"`) |
| `widget.label.font_weight` | string\|null | Label font weight (e.g. `"600"`) |
| `widget.text` | object | Text/value styling element group |
| `widget.text.color` | string\|null | Value text color |
| `widget.text.color:hover` | string\|null | Value text color on hover |
| `widget.text.background` | string\|null | Value background color |
| `widget.text.background:hover` | string\|null | Value background on hover |
| `widget.text.padding` | number | Value padding in px (default: `0`) |
| `widget.text.font_size` | string\|null | Value font size |
| `widget.text.font_weight` | string\|null | Value font weight |
| `widget.config` | object | Configuration element group |
| `widget.config.collection_code` | string | Collection code |
| `widget.config.field_code` | string | Field code to display |
| `widget.config.entry_source` | string | Entry source: `"static"` or `"url"` (default: `"static"`) |
| `widget.config.entry_id` | string | Specific entry ID (when `entry_source` is `"static"`) |
| `widget.config.entry_url_segment` | number | URL segment index for `"url"` entry source (default: `1`) |
| `widget.config.display_as` | string | HTML tag: `"p"`, `"h1"`-`"h6"`, `"span"`, `"image"`, `"gallery"` (default: `"p"`) |
| `widget.config.value_source` | string | Value source: `"static"` or `"dynamic"` (default: `"dynamic"`) |
| `widget.config.field_type` | string\|null | Field type (e.g. `"text"`, `"date"`, `"image"`) |
| `widget.config.link_to_entry` | boolean | Wrap value as link to entry page (default: `false`) |
| `widget.config.date_format` | string | Date format: `"full"`, `"short"`, `"relative"`, `"custom"` (default: `"full"`) |
| `widget.config.custom_date_format` | string\|null | Custom date format string (e.g. `"DD.MM.YYYY HH:mm"`) |
| `widget.config.show_time` | boolean | Show time for datetime fields (default: `true`) |
| `widget.config.preview_entry_id` | string | Preview entry ID for editor |
| `widget.config.button_style` | string | Button style: `"primary"`, `"secondary"`, `"outline"` (default: `"primary"`) |
| `widget.config.button_size` | string | Button size: `"sm"`, `"md"`, `"lg"` (default: `"md"`) |
| `widget.config.image_width` | string\|null | Image width (e.g. `"200px"`, `"100%"`) |
| `widget.config.image_height` | string\|null | Image height (e.g. `"150px"`) |
| `widget.config.image_object_fit` | string | Image object-fit: `"contain"`, `"cover"`, `"fill"` (default: `"contain"`) |
| `widget.config.image_border_radius` | number | Image border radius in px (default: `0`) |
| `settings` | object | [Shared widget settings](shared-settings.md) |

### Server-Side Enrichment Fields

When `config.value_source` is `"dynamic"`, `config.entry_source` is `"static"`, and the API can resolve the entry, these fields are added to `widget.config`:

| Property | Type | Description |
|----------|------|-------------|
| `widget.config.value` | any | Pre-extracted field value from the entry (multilingual object or scalar) |
| `widget.config.entry_url` | string\|null | Resolved entry URL based on collection route configuration |

## Example Response

```json
{
  "widget_type": "data-field",
  "uuid": "df-001",
  "widget": {
    "icon": {
      "show": false,
      "icon": "fa-solid fa-star",
      "position": "left",
      "size": 24,
      "color": "#50a5f1",
      "color:hover": null,
      "background": "transparent",
      "background:hover": null,
      "padding": 0,
      "border_radius": 0,
      "gap": 12
    },
    "label": {
      "position": "above",
      "html": { "en": "Full Name", "pl": "Imie i nazwisko" },
      "background": "#f8d775",
      "background:hover": null,
      "color": "#000000",
      "color:hover": null,
      "padding": 8,
      "font_size": "14px",
      "font_weight": "600"
    },
    "text": {
      "color": null,
      "color:hover": null,
      "background": null,
      "background:hover": null,
      "padding": 0,
      "font_size": null,
      "font_weight": null
    },
    "config": {
      "collection_code": "team",
      "field_code": "name",
      "entry_source": "static",
      "entry_id": "entry-uuid-123",
      "entry_url_segment": 1,
      "display_as": "h2",
      "value_source": "dynamic",
      "field_type": "text",
      "link_to_entry": false,
      "date_format": "full",
      "custom_date_format": null,
      "show_time": true,
      "preview_entry_id": "",
      "button_style": "primary",
      "button_size": "md",
      "image_width": null,
      "image_height": null,
      "image_object_fit": "contain",
      "image_border_radius": 0,
      "value": { "en": "John Smith", "pl": "Jan Kowalski" },
      "entry_url": "/team/john-smith"
    }
  },
  "settings": {
    "padding_top": 0,
    "padding_right": 0,
    "padding_bottom": 0,
    "padding_left": 0,
    "border_radius": 0,
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Value Source

| Value | Description |
|-------|-------------|
| `static` | Use static multilingual value from widget |
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
function renderDataField(widget, language) {
  const { icon, label, text, config } = widget.widget;

  // Build label HTML
  let labelHtml = '';
  if (label.position !== 'hidden' && label.html) {
    const labelText = label.html[language] || label.html.en || '';
    if (labelText) {
      labelHtml = `<span class="data-field__label" style="
        color: ${label.color || 'inherit'};
        background: ${label.background || 'transparent'};
        padding: ${label.padding}px;
        font-size: ${label.font_size || 'inherit'};
        font-weight: ${label.font_weight || 'inherit'};
      ">${labelText}</span>`;
    }
  }

  // Build icon HTML
  let iconHtml = '';
  if (icon.show && icon.icon) {
    iconHtml = `<i class="${icon.icon}" style="
      font-size: ${icon.size}px;
      color: ${icon.color || 'inherit'};
      background: ${icon.background || 'transparent'};
      padding: ${icon.padding}px;
      border-radius: ${icon.border_radius}px;
    "></i>`;
  }

  // Get value (from enriched data or fetch client-side)
  let content = '';
  if (config.value !== undefined) {
    const val = config.value;
    content = typeof val === 'object' && !Array.isArray(val)
      ? (val[language] || val.en || '')
      : String(val || '');
  }

  const tag = config.display_as || 'p';

  return `
    <div class="data-field" style="
      color: ${text.color || 'inherit'};
      background: ${text.background || 'transparent'};
      padding: ${text.padding}px;
      font-size: ${text.font_size || 'inherit'};
      font-weight: ${text.font_weight || 'inherit'};
    ">
      ${label.position === 'above' ? labelHtml : ''}
      <div style="display: flex; align-items: center; gap: ${icon.gap}px;">
        ${icon.position === 'left' ? iconHtml : ''}
        ${label.position === 'inline' ? labelHtml : ''}
        <${tag}>${content}</${tag}>
        ${icon.position === 'right' ? iconHtml : ''}
      </div>
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
- **Image fields**: Display images with custom sizing and fit
