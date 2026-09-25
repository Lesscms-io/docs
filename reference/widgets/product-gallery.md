# Product Gallery Widget

Displays product images in a gallery format on product detail pages. Supports both single and multiple image fields, with thumbnail navigation and optional image zoom. Used on e-commerce product pages to showcase product variations and details.

## Widget Type

```
product-gallery
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"product-gallery"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget properties container |
| `widget.config` | object | Configuration element group |
| `widget.config.field_image` | string | Product field path for single image (e.g., `"image"`, `"featured_image"`) |
| `widget.config.field_images` | string | Product field path for multiple images array (e.g., `"images"`, `"gallery"`) |
| `widget.config.border_radius` | number | Corner radius in pixels (default: `8`) |
| `widget.config.thumb_size` | number | Thumbnail width/height in pixels (default: `70`) |
| `widget.config.enable_zoom` | boolean | Enable image zoom on hover/click (default: `true`) |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "product-gallery",
  "uuid": "product-gallery-456",
  "widget": {
    "config": {
      "field_image": "image",
      "field_images": "images",
      "border_radius": 8,
      "thumb_size": 70,
      "enable_zoom": true
    }
  },
  "settings": {
    "padding_top": 0,
    "padding_bottom": 0,
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Field Configuration

Both `field_image` and `field_images` can be provided:
- If `field_images` is set, it takes priority and displays all images with thumbnails
- If only `field_image` is set, displays single image (no thumbnails)
- If both are empty, widget renders as empty

## Gallery Layout

- **Main image**: Large display of the current/active image
- **Thumbnails**: Smaller preview strip below main image (if multiple images)
- **Navigation**: Click thumbnails to switch main image view
- **Zoom**: Hover or click main image to see enlarged version (if enabled)

## Responsive Behavior

- **Desktop**: Main image above thumbnail strip
- **Mobile**: Main image full-width, thumbnails scroll horizontally if needed

## Image Optimization

Images are served via the LessCMS image proxy for automatic optimization:
- Responsive images with `srcset` for different screen sizes
- Lazy loading for thumbnails
- WebP format support for modern browsers
