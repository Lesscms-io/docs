# Gallery Widget

A multi-image gallery widget with grid and carousel display options.

## Widget Type

```
gallery
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"gallery"` |
| `uuid` | string | Unique widget identifier |
| `config` | object | Widget configuration |
| `config.content_source` | string | `"static"` or `"dynamic"` |
| `config.type` | string | Display type: `"grid"` or `"carousel"` |
| `config.columns` | number | Number of columns (for grid) |
| `config.gap` | number | Gap between images in pixels |
| `config.aspect` | string | Image aspect ratio: `"square"`, `"16:9"`, etc. |
| `config.show_arrows` | boolean | Show navigation arrows (carousel) |
| `config.show_dots` | boolean | Show navigation dots (carousel) |
| `config.carousel_style` | string | Carousel style variant |
| `config.images` | array | Array of image objects (when static) |
| `config.collection_code` | string | Collection code (when dynamic) |
| `config.field_code` | string | Field code (when dynamic) |
| `config.entry_id` | string | Entry ID (when dynamic) |
| `settings` | object | Style settings (optional) |

## Example Response (Static Grid)

```json
{
  "widget_type": "gallery",
  "uuid": "gallery-123",
  "config": {
    "content_source": "static",
    "type": "grid",
    "columns": 4,
    "gap": 8,
    "aspect": "square",
    "show_arrows": true,
    "show_dots": true,
    "carousel_style": "default",
    "images": [
      { "url": "https://cdn.example.com/img1.jpg", "alt": "Image 1" },
      { "url": "https://cdn.example.com/img2.jpg", "alt": "Image 2" },
      { "url": "https://cdn.example.com/img3.jpg", "alt": "Image 3" },
      { "url": "https://cdn.example.com/img4.jpg", "alt": "Image 4" }
    ]
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
  "widget_type": "gallery",
  "uuid": "gallery-456",
  "config": {
    "content_source": "dynamic",
    "type": "carousel",
    "columns": 3,
    "gap": 16,
    "aspect": "16:9",
    "show_arrows": true,
    "show_dots": true,
    "carousel_style": "default",
    "collection_code": "products",
    "field_code": "gallery",
    "entry_id": "product-1"
  },
  "settings": {}
}
```

## Display Types

| Value | Description |
|-------|-------------|
| `grid` | Display images in a responsive grid |
| `carousel` | Display images in a sliding carousel |

## Usage Example

```javascript
function renderGallery(widget) {
  const { type, columns, gap, images } = widget.config;

  if (!images || images.length === 0) return '';

  if (type === 'grid') {
    const imageItems = images.map(img => `
      <div class="gallery-item">
        <img src="${img.url}" alt="${img.alt || ''}" loading="lazy">
      </div>
    `).join('');

    return `
      <div class="gallery-grid" style="
        display: grid;
        grid-template-columns: repeat(${columns}, 1fr);
        gap: ${gap}px;
      ">
        ${imageItems}
      </div>
    `;
  } else {
    // Carousel implementation
    return `<div class="gallery-carousel" data-images='${JSON.stringify(images)}'></div>`;
  }
}
```
