# Gallery Widget

A multi-image gallery widget supporting grid and carousel display modes, with static or dynamic (collection-based) image sources and optional lightbox.

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
| `config.content_source` | string | Image source: `"static"` or `"dynamic"` (default: `"static"`) |
| `config.type` | string | Display type: `"grid"` or `"carousel"` (default: `"grid"`) |
| `config.columns` | number | Number of grid columns (default: 3) |
| `config.gap` | number | Gap between images in pixels (default: 8) |
| `config.aspect` | string | Image aspect ratio: `"square"`, `"landscape"`, `"portrait"`, etc. (default: `"square"`) |
| `config.show_arrows` | boolean | Show navigation arrows for carousel (default: true) |
| `config.show_dots` | boolean | Show dot indicators for carousel (default: true) |
| `config.carousel_style` | string | Carousel style preset (default: `"default"`) |
| `config.enable_lightbox` | boolean | Enable lightbox on click (default: false) |
| `config.images` | array | Array of image objects (static mode only) |
| `config.images[].url` | string | Image URL |
| `config.images[].alt` | string | Image alt text |
| `config.collection_code` | string\|null | Collection code for dynamic images (dynamic mode only) |
| `config.field_code` | string\|null | Field code containing images (dynamic mode only) |
| `config.entry_id` | string\|null | Specific entry ID to pull images from (dynamic mode only) |
| `settings` | object | Style settings (optional) |

## Example Response (Static Images)

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
    "enable_lightbox": true,
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

## Example Response (Dynamic Images from Collection)

```json
{
  "widget_type": "gallery",
  "uuid": "gallery-456",
  "config": {
    "content_source": "dynamic",
    "type": "carousel",
    "columns": 3,
    "gap": 8,
    "aspect": "landscape",
    "show_arrows": true,
    "show_dots": true,
    "carousel_style": "default",
    "enable_lightbox": false,
    "collection_code": "portfolio",
    "field_code": "gallery_images",
    "entry_id": null
  }
}
```

## Display Types

| Value | Description |
|-------|-------------|
| `"grid"` | Grid layout with configurable columns |
| `"carousel"` | Carousel/slider with arrows and dots |

## Aspect Ratios

| Value | Description |
|-------|-------------|
| `"square"` | 1:1 aspect ratio |
| `"landscape"` | Landscape aspect ratio |
| `"portrait"` | Portrait aspect ratio |

## Usage Example

```javascript
function renderGallery(widget) {
  const { content_source, type, images, columns, gap, aspect, enable_lightbox, show_arrows, show_dots } = widget.config;

  // Dynamic mode: images must be fetched from collection API separately
  if (content_source === 'dynamic') {
    const { collection_code, field_code, entry_id } = widget.config;
    return `<div class="gallery-dynamic" data-collection="${collection_code}" data-field="${field_code}" data-entry="${entry_id || ''}"></div>`;
  }

  if (!images || images.length === 0) return '';

  const imageItems = images.map(img => `
    <div class="gallery-item" style="aspect-ratio: ${aspect === 'square' ? '1/1' : aspect === 'landscape' ? '16/9' : '9/16'};">
      <img
        src="${img.url}"
        alt="${img.alt || ''}"
        loading="lazy"
        ${enable_lightbox ? `style="cursor: pointer;" onclick="openLightbox('${img.url}')"` : ''}
      >
    </div>
  `).join('');

  if (type === 'carousel') {
    return `
      <div class="gallery-carousel" data-arrows="${show_arrows}" data-dots="${show_dots}">
        ${imageItems}
      </div>
    `;
  }

  return `
    <div class="gallery-grid" style="
      display: grid;
      grid-template-columns: repeat(${columns}, 1fr);
      gap: ${gap}px;
    ">
      ${imageItems}
    </div>
  `;
}
```
