# Gallery Widget

An image gallery widget with grid layout and optional lightbox.

## Widget Type

```
gallery
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"gallery"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget data |
| `widget.images` | array | Array of image objects (each with `url` and `alt` properties) |
| `widget.columns` | number\|string | Number of grid columns (default: 3) |
| `widget.enable_lightbox` | boolean | Enable lightbox on click (default: false) |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "gallery",
  "uuid": "gallery-123",
  "widget": {
    "images": [
      { "url": "https://cdn.example.com/img1.jpg", "alt": "Image 1" },
      { "url": "https://cdn.example.com/img2.jpg", "alt": "Image 2" },
      { "url": "https://cdn.example.com/img3.jpg", "alt": "Image 3" },
      { "url": "https://cdn.example.com/img4.jpg", "alt": "Image 4" }
    ],
    "columns": 4,
    "enable_lightbox": true
  },
  "settings": {
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Usage Example

```javascript
function renderGallery(widget) {
  const { images, columns, enable_lightbox } = widget.widget;

  if (!images || images.length === 0) return '';

  const imageItems = images.map((img, index) => `
    <div class="gallery-item" style="aspect-ratio: 1/1;">
      <img
        src="${img.url}"
        alt="${img.alt || ''}"
        loading="lazy"
        ${enable_lightbox ? `style="cursor: pointer;" onclick="openLightbox(${index})"` : ''}
      >
    </div>
  `).join('');

  return `
    <div class="gallery-grid" style="
      display: grid;
      grid-template-columns: repeat(${columns || 3}, 1fr);
      gap: 8px;
    ">
      ${imageItems}
    </div>
  `;
}
```
