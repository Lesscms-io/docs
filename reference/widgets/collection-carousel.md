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
| `widget` | object | Widget data |
| `widget.collection_code` | string\|null | Collection code to display |
| `widget.posts_count` | number | Maximum entries to display (default: 6) |
| `widget.slides_per_view` | number | Visible slides (default: 3) |
| `widget.autoplay` | boolean | Enable auto-advance (default: true) |
| `widget.autoplay_interval` | number | Autoplay interval in ms (default: 5000) |
| `widget.show_arrows` | boolean | Display navigation arrows (default: true) |
| `widget.show_dots` | boolean | Display navigation dots (default: true) |
| `widget.title_field` | string\|null | Field code for title |
| `widget.excerpt_field` | string\|null | Field code for excerpt |
| `widget.image_field` | string\|null | Field code for image |
| `widget.show_title` | boolean | Display title (default: true) |
| `widget.show_excerpt` | boolean | Display excerpt (default: true) |
| `widget.exclude_current_entry` | boolean | Exclude current entry from carousel results (default: false) |
| `widget.route_uuid` | string\|null | Route UUID for entry detail pages |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "collection-carousel",
  "uuid": "carousel-123",
  "widget": {
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
    "show_excerpt": true,
    "exclude_current_entry": false,
    "route_uuid": null
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
  } = widget.widget;

  // Entries may be pre-fetched by the API (server-side enrichment).
  // If widget.entries exists, use it directly. Otherwise, fetch client-side.
  const entries = widget.widget.entries || []; // Fallback: fetch from /api/:project_code/collections/:collection_code

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
        <button class="carousel-prev" aria-label="Previous">&#8249;</button>
        <button class="carousel-next" aria-label="Next">&#8250;</button>
      ` : ''}
      ${show_dots ? `<div class="carousel-dots"></div>` : ''}
    </div>
  `;
}
```
