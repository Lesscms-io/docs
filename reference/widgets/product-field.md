# Product Field Widget

Displays a single field or property from the current product (e.g., name, SKU, description). Used on product detail pages to render dynamic product data. Supports multiple display formats (text, heading, image, gallery, price, badge) with optional label and styling options.

## Widget Type

```
product-field
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"product-field"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget properties container |
| `widget.config` | object | Configuration element group |
| `widget.config.field_path` | string | Dot-notation path to the product field (e.g., `"name"`, `"price"`, `"category.name"`, `"sku"`) |
| `widget.config.display_as` | string | How to render the field: `"text"`, `"heading"`, `"h1"`–`"h6"`, `"price"`, `"image"`, `"gallery"`, `"html"`, `"badge"` (default: `"text"`) |
| `widget.config.hide_when_empty` | boolean | Hide widget if field value is empty/null (default: `true`) |
| `widget.label` | object | Label element group |
| `widget.label.text` | string | Optional label displayed before field value |
| `widget.label.color` | string\|null | Label text color (color variable or hex) |
| `widget.label.show` | boolean | Display the label (default: `false` or `true` if label.text provided) |
| `widget.text` | object | Text/value element group |
| `widget.text.color` | string\|null | Field value text color (color variable or hex, inherits from body if null) |
| `widget.text.font_size` | number\|null | Font size in pixels (inherits if null) |
| `widget.text.font_weight` | string\|null | Font weight: `"400"`, `"500"`, `"600"`, `"700"`, `"800"`, or null for default |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "product-field",
  "uuid": "product-field-123",
  "widget": {
    "config": {
      "field_path": "name",
      "display_as": "heading",
      "hide_when_empty": true
    },
    "label": {
      "text": "",
      "color": "var:muted",
      "show": false
    },
    "text": {
      "color": null,
      "font_size": null,
      "font_weight": null
    }
  },
  "settings": {
    "padding_top": 8,
    "padding_bottom": 8,
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Display Format Details

| Format | Use Case | Output |
|--------|----------|--------|
| `text` | Simple text content | Plain text or multilingual string (resolved to current language) |
| `heading` | Default heading size | `<h3>` with field value |
| `h1`–`h6` | Explicit heading level | `<h1>` through `<h6>` with field value |
| `price` | Product price | Formatted price with currency symbol |
| `image` | Single image field | `<img>` tag with image URL from field |
| `gallery` | Array of images | Grid or carousel of images |
| `html` | Rich text/HTML | Raw HTML rendering (field must contain valid HTML) |
| `badge` | Tag/label display | Badge-styled element (typically for variant names, tags) |

## Field Paths

Use dot notation to access nested fields:
- `name` – Product name
- `price` – Regular price
- `compare_at_price` – Original/list price (for showing discounts)
- `description` – Rich text description
- `category.name` – Category name (nested)
- `sku` – Stock keeping unit
- Any custom product field defined in your shop schema

## Multilingual Handling

For multilingual product fields, the widget automatically resolves to the current page language. If a field is not translated, it falls back to the primary language.
