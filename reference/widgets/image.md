# Image Widget

An image display widget supporting both static images and dynamic content from collections.

## Widget Type

```
image
```

## Data Properties

| Property | Type | Global | Description |
|----------|------|--------|-------------|
| `image_source` | string | Yes | Image source: `static` or `dynamic` |
| `image` | object | Yes | Image data (when static) |

### Image Object Structure (static)

| Property | Type | Description |
|----------|------|-------------|
| `url` | string | Image URL |
| `alt` | object | Alt text (multilingual) |
| `title` | object | Title text (multilingual) |

## Example Response (Static)

```json
{
  "widget_type": "image",
  "uuid": "img-123",
  "data": {
    "image_source": "static",
    "image": {
      "url": "https://cdn.example.com/images/hero.jpg",
      "alt": {
        "en": "Beautiful landscape",
        "pl": "Piękny krajobraz"
      },
      "title": {
        "en": "Mountain View",
        "pl": "Widok na góry"
      }
    }
  },
  "settings": {
    "textAlign": "center",
    "width": "100%",
    "maxWidth": 800,
    "borderRadius": 8
  }
}
```

## Example Response (Dynamic)

When `image_source` is `dynamic`, the image is fetched from a collection entry:

```json
{
  "widget_type": "image",
  "uuid": "img-456",
  "data": {
    "image_source": "dynamic",
    "collection_code": "products",
    "field_code": "main_image",
    "entry_source": "url",
    "entry_url_segment": 1
  },
  "settings": {}
}
```

## Usage Example

```javascript
// Render image widget
function renderImage(widget, language) {
  const { image_source, image } = widget.data;
  const settings = widget.settings || {};

  if (image_source !== 'static' || !image?.url) {
    return '';
  }

  const alt = image.alt?.[language] || image.alt?.en || '';
  const title = image.title?.[language] || image.title?.en || '';

  const styles = [];
  if (settings.width) styles.push(`width: ${settings.width}`);
  if (settings.maxWidth) styles.push(`max-width: ${settings.maxWidth}px`);
  if (settings.borderRadius) styles.push(`border-radius: ${settings.borderRadius}px`);

  return `
    <img
      src="${image.url}"
      alt="${alt}"
      ${title ? `title="${title}"` : ''}
      style="${styles.join('; ')}"
      loading="lazy"
    >
  `;
}
```

## Responsive Images

For optimal performance, consider using responsive images with srcset:

```javascript
function renderResponsiveImage(widget, language) {
  const { image } = widget.data;

  if (!image?.url) return '';

  const alt = image.alt?.[language] || '';
  const baseUrl = image.url;

  // Assuming your CDN supports image transformations
  return `
    <picture>
      <source
        media="(max-width: 768px)"
        srcset="${baseUrl}?w=768"
      >
      <source
        media="(max-width: 1200px)"
        srcset="${baseUrl}?w=1200"
      >
      <img
        src="${baseUrl}"
        alt="${alt}"
        loading="lazy"
      >
    </picture>
  `;
}
```
