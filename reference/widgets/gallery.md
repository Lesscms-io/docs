# Gallery Widget

A multi-image gallery grid with customizable columns.

## Widget Type

```
gallery
```

## Data Properties

| Property | Type | Global | Description |
|----------|------|--------|-------------|
| `images` | array | Yes | Array of image objects |
| `columns` | string | Yes | Number of columns: `2`, `3`, `4`, `5` |

### Image Object Structure

Each image in the `images` array contains:

| Property | Type | Description |
|----------|------|-------------|
| `url` | string | Image URL |
| `alt` | object | Alt text (multilingual) |
| `title` | object | Title text (multilingual) |

## Example Response

```json
{
  "widget_type": "gallery",
  "uuid": "gallery-123",
  "data": {
    "images": [
      {
        "url": "https://cdn.example.com/img1.jpg",
        "alt": { "en": "Image 1", "pl": "Obraz 1" }
      },
      {
        "url": "https://cdn.example.com/img2.jpg",
        "alt": { "en": "Image 2", "pl": "Obraz 2" }
      },
      {
        "url": "https://cdn.example.com/img3.jpg",
        "alt": { "en": "Image 3", "pl": "Obraz 3" }
      },
      {
        "url": "https://cdn.example.com/img4.jpg",
        "alt": { "en": "Image 4", "pl": "Obraz 4" }
      }
    ],
    "columns": "4"
  },
  "settings": {
    "paddingTop": 20,
    "paddingBottom": 20
  }
}
```

## Column Values

| Value | Description |
|-------|-------------|
| `2` | 2-column grid (50% each) |
| `3` | 3-column grid (33.33% each) |
| `4` | 4-column grid (25% each) |
| `5` | 5-column grid (20% each) |

## Usage Example

```javascript
// Render gallery widget
function renderGallery(widget, language) {
  const { images, columns } = widget.data;
  const numColumns = parseInt(columns) || 3;

  if (!images || images.length === 0) {
    return '';
  }

  const imageItems = images.map((img, index) => {
    const alt = img.alt?.[language] || img.alt?.en || `Image ${index + 1}`;
    return `
      <div class="gallery-item">
        <img src="${img.url}" alt="${alt}" loading="lazy">
      </div>
    `;
  }).join('');

  return `
    <div class="gallery" style="
      display: grid;
      grid-template-columns: repeat(${numColumns}, 1fr);
      gap: 16px;
    ">
      ${imageItems}
    </div>
  `;
}
```

## Lightbox Integration

For a better user experience, consider adding a lightbox:

```javascript
function renderGalleryWithLightbox(widget, language) {
  const { images, columns } = widget.data;

  const imageItems = images.map((img, index) => {
    const alt = img.alt?.[language] || '';
    return `
      <a href="${img.url}" data-lightbox="gallery" data-alt="${alt}">
        <img src="${img.url}?w=400" alt="${alt}" loading="lazy">
      </a>
    `;
  }).join('');

  return `
    <div class="gallery gallery-${columns}-columns">
      ${imageItems}
    </div>
  `;
}
```

## Responsive Grid

```css
.gallery {
  display: grid;
  gap: 16px;
}

/* Desktop: use configured columns */
.gallery-4-columns { grid-template-columns: repeat(4, 1fr); }
.gallery-3-columns { grid-template-columns: repeat(3, 1fr); }
.gallery-2-columns { grid-template-columns: repeat(2, 1fr); }

/* Tablet: max 3 columns */
@media (max-width: 1024px) {
  .gallery-4-columns,
  .gallery-5-columns {
    grid-template-columns: repeat(3, 1fr);
  }
}

/* Mobile: max 2 columns */
@media (max-width: 768px) {
  .gallery {
    grid-template-columns: repeat(2, 1fr);
  }
}
```
