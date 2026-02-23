# Image Widget

An image widget with optional dynamic content from collections.

## Widget Type

```
image
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"image"` |
| `uuid` | string | Unique widget identifier |
| `config` | object | Widget configuration |
| `config.image_source` | string | `"static"` or `"dynamic"` |
| `config.image_style` | string | Image style preset: `"none"`, `"rounded"`, `"rounded-lg"`, `"circle"`, `"shadow-sm"`, `"shadow-lg"`, `"rounded-shadow"`, `"border"`, `"border-rounded"` |
| `config.collection_code` | string | Collection code (when dynamic) |
| `config.field_code` | string | Field code (when dynamic) |
| `config.entry_id` | string | Entry ID (when dynamic) |
| `content` | object | Widget content (when static) |
| `content.url` | string | Image URL |
| `content.alt` | object | Multilingual alt text |
| `settings` | object | Style settings (optional) |

## Example Response (Static)

```json
{
  "widget_type": "image",
  "uuid": "img-123",
  "config": {
    "image_source": "static",
    "image_style": "rounded-shadow"
  },
  "content": {
    "url": "https://cdn.example.com/images/hero.jpg",
    "alt": {
      "en": "Company headquarters",
      "pl": "Siedziba firmy"
    }
  },
  "settings": {
    "horizontalAlign": "center",
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
  "widget_type": "image",
  "uuid": "img-456",
  "config": {
    "image_source": "dynamic",
    "image_style": "none",
    "collection_code": "products",
    "field_code": "main_image",
    "entry_id": "product-123"
  },
  "settings": {
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Content Source

| Value | Description |
|-------|-------------|
| `static` | Use `content.url` and `content.alt` directly |
| `dynamic` | Fetch image from collection field |

## Image Style Presets

| Value | Description | CSS |
|-------|-------------|-----|
| `none` | No style (default) | — |
| `rounded` | Rounded corners | `border-radius: 12px` |
| `rounded-lg` | Large rounded corners | `border-radius: 24px` |
| `circle` | Circle / ellipse | `border-radius: 50%` |
| `shadow-sm` | Light shadow | `box-shadow: 0 2px 8px rgba(0,0,0,0.1)` |
| `shadow-lg` | Heavy shadow | `box-shadow: 0 8px 24px rgba(0,0,0,0.2)` |
| `rounded-shadow` | Rounded + shadow | `border-radius: 12px; box-shadow: 0 4px 12px rgba(0,0,0,0.15)` |
| `border` | Border | `border: 2px solid #e0e0e0` |
| `border-rounded` | Border + rounded | `border: 2px solid #e0e0e0; border-radius: 12px` |

## Usage Example

```javascript
async function renderImage(widget, language, entryData, api) {
  const { image_source, image_style, collection_code, field_code, entry_id } = widget.config;
  let url = '';
  let alt = '';

  if (image_source === 'dynamic') {
    // Fetch from collection or use provided entry data
    if (entryData) {
      url = entryData[field_code] || '';
    } else if (collection_code && entry_id) {
      const entry = await api.getEntry(collection_code, entry_id);
      url = entry.data[field_code] || '';
    }
  } else {
    url = widget.content?.url || '';
    alt = widget.content?.alt?.[language] || widget.content?.alt?.en || '';
  }

  // Apply image style preset
  const styleMap = {
    'none': '',
    'rounded': 'border-radius: 12px',
    'rounded-lg': 'border-radius: 24px',
    'circle': 'border-radius: 50%',
    'shadow-sm': 'box-shadow: 0 2px 8px rgba(0,0,0,0.1)',
    'shadow-lg': 'box-shadow: 0 8px 24px rgba(0,0,0,0.2)',
    'rounded-shadow': 'border-radius: 12px; box-shadow: 0 4px 12px rgba(0,0,0,0.15)',
    'border': 'border: 2px solid #e0e0e0',
    'border-rounded': 'border: 2px solid #e0e0e0; border-radius: 12px'
  };
  const style = styleMap[image_style] || '';

  return `<img src="${url}" alt="${alt}" class="img-fluid" style="${style}" />`;
}
```
