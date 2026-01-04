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
    "image_source": "static"
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

## Usage Example

```javascript
async function renderImage(widget, language, entryData, api) {
  const { image_source, collection_code, field_code, entry_id } = widget.config;
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

  return `<img src="${url}" alt="${alt}" class="img-fluid" />`;
}
```
