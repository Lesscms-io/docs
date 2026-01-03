# Key-Value Widget

A label-value pair display widget, supporting both static values and dynamic content from collections.

## Widget Type

```
key-value
```

## Data Properties

| Property | Type | Global | Description |
|----------|------|--------|-------------|
| `label` | object | No | Label text (multilingual) |
| `value_source` | string | Yes | Value source: `static` or `dynamic` |
| `static_value` | object | No | Static value text (multilingual) |
| `collection_code` | string | Yes | Collection code (when dynamic) |
| `entry_id` | string | Yes | Entry ID (when dynamic) |
| `value_field` | string | Yes | Field code to display (when dynamic) |
| `layout` | string | Yes | Layout: `left`, `right`, `top`, `bottom` |
| `vertical_align` | string | Yes | Vertical alignment: `top`, `center`, `bottom` |
| `label_width` | number | Yes | Label width percentage (for horizontal layouts) |
| `gap` | number | Yes | Gap between label and value (pixels) |
| `label_background` | string | Yes | Label background color |
| `label_color` | string | Yes | Label text color |
| `label_padding` | number | Yes | Label padding (pixels) |
| `label_font_size` | number | Yes | Label font size (pixels) |
| `label_font_weight` | string | Yes | Label font weight: `400`, `500`, `600`, `700` |
| `value_background` | string | Yes | Value background color |
| `value_color` | string | Yes | Value text color |
| `value_padding` | number | Yes | Value padding (pixels) |
| `value_font_size` | number | Yes | Value font size (pixels) |
| `value_font_weight` | string | Yes | Value font weight: `400`, `500`, `600`, `700` |

## Example Response (Static)

```json
{
  "widget_type": "key-value",
  "uuid": "kv-123",
  "data": {
    "label": {
      "en": "Price",
      "pl": "Cena"
    },
    "value_source": "static",
    "static_value": {
      "en": "$99.99",
      "pl": "399,99 zł"
    },
    "layout": "left",
    "label_width": 30,
    "gap": 8,
    "label_font_weight": "600",
    "value_font_weight": "400"
  },
  "settings": {}
}
```

## Example Response (Dynamic)

```json
{
  "widget_type": "key-value",
  "uuid": "kv-456",
  "data": {
    "label": {
      "en": "Author",
      "pl": "Autor"
    },
    "value_source": "dynamic",
    "collection_code": "articles",
    "entry_id": "entry-uuid-123",
    "value_field": "author_name",
    "layout": "top",
    "label_font_weight": "600"
  },
  "settings": {}
}
```

## Layout Values

| Value | Description |
|-------|-------------|
| `left` | Label on the left, value on the right |
| `right` | Label on the right, value on the left |
| `top` | Label above the value |
| `bottom` | Label below the value |

## Font Weight Values

| Value | Description |
|-------|-------------|
| `400` | Normal |
| `500` | Medium |
| `600` | Semibold |
| `700` | Bold |

## Usage Example

```javascript
// Render key-value widget
async function renderKeyValue(widget, language, api) {
  const {
    label, value_source, static_value,
    collection_code, entry_id, value_field,
    layout, label_width, gap
  } = widget.data;

  const labelText = label?.[language] || label?.en || '';
  let valueText = '';

  if (value_source === 'dynamic' && collection_code && entry_id) {
    const entry = await api.getEntry(collection_code, entry_id);
    valueText = entry.data[value_field]?.[language] || '';
  } else {
    valueText = static_value?.[language] || static_value?.en || '';
  }

  const isHorizontal = ['left', 'right'].includes(layout);

  return `
    <div class="key-value" style="
      display: flex;
      flex-direction: ${isHorizontal ? 'row' : 'column'};
      gap: ${gap}px;
    ">
      <span class="label" style="${isHorizontal ? `width: ${label_width}%` : ''}">
        ${labelText}
      </span>
      <span class="value">${valueText}</span>
    </div>
  `;
}
```
