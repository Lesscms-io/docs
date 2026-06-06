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
| `widget.images` | array | Array of media items. Each item: `url`, `alt`, `type` (`"image"` default, or `"video"`), and `poster` (optional poster URL for videos). Videos render as `<video>` and play in the lightbox. |
| `widget.columns` | number\|string | Number of grid columns (default: 3) |
| `widget.enable_lightbox` | boolean | Enable lightbox on click (default: false) |
| `widget.type` | string | Gallery type: `"grid"`, `"masonry"`, `"carousel"`, `"mosaic"` |
| `widget.gap` | number | Gap between gallery items in pixels |
| `widget.aspect` | string | Image aspect ratio (e.g. `"16:9"`, `"4:3"`, `"1:1"`, `"auto"`) |
| `widget.mosaic_variant` | string\|null | Mosaic layout variant |
| `widget.carousel_style` | string\|null | Carousel visual style |
| `widget.autoplay` | boolean | Auto-advance carousel slides |
| `widget.interval` | number | Autoplay interval in milliseconds |
| `widget.show_arrows` | boolean | Show navigation arrows |
| `widget.show_dots` | boolean | Show navigation dots |
| `widget.loop` | boolean | Loop carousel slides |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "gallery",
  "uuid": "gallery-123",
  "widget": {
    "images": [
      { "url": "https://cdn.example.com/img1.jpg", "alt": "Image 1", "type": "image" },
      { "url": "https://cdn.example.com/clip.mp4", "alt": "Clip", "type": "video" },
      { "url": "https://cdn.example.com/img3.jpg", "alt": "Image 3", "type": "image" },
      { "url": "https://cdn.example.com/img4.jpg", "alt": "Image 4", "type": "image" }
    ],
    "type": "grid",
    "columns": 4,
    "gap": 8,
    "aspect": "square",
    "enable_lightbox": true,
    "mosaic_variant": "featured",
    "carousel_style": "default",
    "autoplay": true,
    "interval": 3000,
    "show_arrows": true,
    "show_dots": true,
    "loop": true
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

  const imageItems = images.map((img, index) => {
    // Videos: show the first frame with a play overlay; play in the lightbox.
    const isVideo = img.type === 'video';
    // Video tiles are always clickable; images only when the lightbox is enabled.
    const clickable = enable_lightbox || isVideo;
    const media = isVideo
      ? `<video src="${img.url}" ${img.poster ? `poster="${img.poster}"` : ''} preload="metadata" muted playsinline></video>`
      : `<img src="${img.url}" alt="${img.alt || ''}" loading="lazy">`;
    return `
      <div class="gallery-item" style="aspect-ratio: 1/1;"
        ${clickable ? `onclick="openLightbox(${index})" style="cursor: pointer;"` : ''}>
        ${media}
      </div>
    `;
  }).join('');

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
