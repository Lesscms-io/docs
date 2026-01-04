# Collection Carousel Widget

A carousel/slider display of collection entries.

## Widget Type

```
collection-carousel
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"collection-carousel"` |
| `uuid` | string | Unique widget identifier |
| `config` | object | Widget configuration |
| `config.collection_code` | string\|null | Collection code to display |
| `config.posts_count` | number | Maximum entries to display (default: 6) |
| `config.slides_per_view` | number | Visible slides (default: 3) |
| `config.autoplay` | boolean | Enable auto-advance (default: true) |
| `config.autoplay_interval` | number | Autoplay interval in ms (default: 5000) |
| `config.show_arrows` | boolean | Display navigation arrows (default: true) |
| `config.show_dots` | boolean | Display navigation dots (default: true) |
| `config.title_field` | string\|null | Field code for title |
| `config.excerpt_field` | string\|null | Field code for excerpt |
| `config.image_field` | string\|null | Field code for image |
| `config.show_title` | boolean | Display title (default: true) |
| `config.show_excerpt` | boolean | Display excerpt (default: true) |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "collection-carousel",
  "uuid": "carousel-123",
  "config": {
    "collection_code": "testimonials",
    "posts_count": 10,
    "slides_per_view": 3,
    "autoplay": true,
    "autoplay_interval": 5000,
    "show_arrows": true,
    "show_dots": true,
    "title_field": "author_name",
    "excerpt_field": "testimonial_text",
    "image_field": "author_photo",
    "show_title": true,
    "show_excerpt": true
  },
  "settings": {
    "paddingTop": 40,
    "paddingBottom": 40,
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Usage Example

```javascript
function renderCollectionCarousel(widget, language) {
  const {
    slides_per_view, autoplay, autoplay_interval,
    show_arrows, show_dots, show_title, show_excerpt,
    title_field, excerpt_field, image_field
  } = widget.config;

  const entries = widget.data?.entries || [];

  if (entries.length === 0) return '';

  const carouselId = `carousel-${widget.uuid}`;

  const slides = entries.map(entry => {
    const title = title_field ? (entry.data[title_field]?.[language] || '') : '';
    const excerpt = excerpt_field ? (entry.data[excerpt_field]?.[language] || '') : '';
    const image = image_field ? entry.data[image_field] : '';

    return `
      <div class="carousel-slide">
        ${image ? `<img src="${image}" alt="${title}" loading="lazy">` : ''}
        ${show_title && title ? `<h4>${title}</h4>` : ''}
        ${show_excerpt && excerpt ? `<p>${excerpt}</p>` : ''}
      </div>
    `;
  }).join('');

  return `
    <div id="${carouselId}"
      class="collection-carousel"
      data-slides-per-view="${slides_per_view}"
      data-autoplay="${autoplay}"
      data-interval="${autoplay_interval}"
    >
      <div class="carousel-track">${slides}</div>
      ${show_arrows ? `
        <button class="carousel-prev" aria-label="Previous">‹</button>
        <button class="carousel-next" aria-label="Next">›</button>
      ` : ''}
      ${show_dots ? `<div class="carousel-dots"></div>` : ''}
    </div>
  `;
}
```
