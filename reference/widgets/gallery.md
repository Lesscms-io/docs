# Gallery Widget

A multi-image gallery widget with grid display and optional lightbox.

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
| `config.images` | array | Array of image objects |
| `config.columns` | string | Number of columns: `"2"`, `"3"`, `"4"`, `"5"` (default: `"3"`) |
| `config.enable_lightbox` | boolean | Enable lightbox on click (default: false) |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "gallery",
  "uuid": "gallery-123",
  "config": {
    "images": [
      { "url": "https://cdn.example.com/img1.jpg", "alt": "Image 1" },
      { "url": "https://cdn.example.com/img2.jpg", "alt": "Image 2" },
      { "url": "https://cdn.example.com/img3.jpg", "alt": "Image 3" },
      { "url": "https://cdn.example.com/img4.jpg", "alt": "Image 4" }
    ],
    "columns": "4",
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

## Columns

| Value | Description |
|-------|-------------|
| `"2"` | 2-column grid |
| `"3"` | 3-column grid |
| `"4"` | 4-column grid |
| `"5"` | 5-column grid |

## Usage Example

```javascript
function renderGallery(widget) {
  const { images, columns, enable_lightbox } = widget.config;

  if (!images || images.length === 0) return '';

  const imageItems = images.map(img => `
    <div class="gallery-item">
      <img
        src="${img.url}"
        alt="${img.alt || ''}"
        loading="lazy"
        ${enable_lightbox ? `style="cursor: pointer;" onclick="openLightbox('${img.url}')"` : ''}
      >
    </div>
  `).join('');

  return `
    <div class="gallery-grid" style="
      display: grid;
      grid-template-columns: repeat(${columns}, 1fr);
      gap: 8px;
    ">
      ${imageItems}
    </div>
  `;
}
```
