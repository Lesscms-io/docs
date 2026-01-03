# Collection Carousel Widget

A carousel/slider display of collection entries.

## Widget Type

```
collection-carousel
```

## Data Properties

| Property | Type | Global | Description |
|----------|------|--------|-------------|
| `collection_code` | string | Yes | Collection code to display |
| `posts_count` | number | Yes | Maximum entries to display |
| `slides_per_view` | string | Yes | Visible slides: `1`, `2`, `3`, `4` |
| `autoplay` | boolean | Yes | Enable automatic slide advancement |
| `autoplay_interval` | number | Yes | Autoplay interval in milliseconds |
| `show_arrows` | boolean | Yes | Display navigation arrows |
| `show_dots` | boolean | Yes | Display navigation dots |
| `title_field` | string | Yes | Field code for entry title |
| `excerpt_field` | string | Yes | Field code for excerpt |
| `image_field` | string | Yes | Field code for image |
| `show_title` | boolean | Yes | Display title |
| `show_excerpt` | boolean | Yes | Display excerpt |

## Example Response

```json
{
  "widget_type": "collection-carousel",
  "uuid": "carousel-123",
  "data": {
    "collection_code": "testimonials",
    "posts_count": 10,
    "slides_per_view": "3",
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
    "paddingBottom": 40
  }
}
```

## Usage Example

```javascript
// Render collection carousel widget
async function renderCollectionCarousel(widget, language, api) {
  const {
    collection_code, posts_count, slides_per_view,
    autoplay, autoplay_interval, show_arrows, show_dots,
    title_field, excerpt_field, image_field,
    show_title, show_excerpt
  } = widget.data;

  // Fetch entries
  const entries = await api.getCollectionEntries(collection_code, {
    limit: posts_count
  });

  if (!entries || entries.length === 0) {
    return '';
  }

  const carouselId = `carousel-${widget.uuid}`;

  const slides = entries.map(entry => {
    const title = entry.data[title_field]?.[language] || '';
    const excerpt = entry.data[excerpt_field]?.[language] || '';
    const image = entry.data[image_field]?.url || '';

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
      <div class="carousel-track">
        ${slides}
      </div>
      ${show_arrows ? `
        <button class="carousel-prev" aria-label="Previous">‹</button>
        <button class="carousel-next" aria-label="Next">›</button>
      ` : ''}
      ${show_dots ? `
        <div class="carousel-dots"></div>
      ` : ''}
    </div>
  `;
}
```

## Swiper.js Integration

```javascript
import Swiper from 'swiper';
import { Navigation, Pagination, Autoplay } from 'swiper/modules';

function initCollectionCarousel(widget) {
  const {
    slides_per_view, autoplay, autoplay_interval,
    show_arrows, show_dots
  } = widget.data;

  return new Swiper(`#carousel-${widget.uuid}`, {
    modules: [Navigation, Pagination, Autoplay],
    slidesPerView: parseInt(slides_per_view) || 3,
    spaceBetween: 24,
    loop: true,
    autoplay: autoplay ? {
      delay: autoplay_interval || 5000,
      disableOnInteraction: false
    } : false,
    navigation: show_arrows ? {
      nextEl: '.carousel-next',
      prevEl: '.carousel-prev'
    } : false,
    pagination: show_dots ? {
      el: '.carousel-dots',
      clickable: true
    } : false,
    breakpoints: {
      320: { slidesPerView: 1 },
      768: { slidesPerView: Math.min(2, slides_per_view) },
      1024: { slidesPerView: slides_per_view }
    }
  });
}
```

## CSS Example

```css
.collection-carousel {
  position: relative;
  overflow: hidden;
}

.carousel-track {
  display: flex;
  transition: transform 0.5s ease;
}

.carousel-slide {
  flex: 0 0 calc(33.333% - 16px);
  margin: 0 12px;
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  padding: 20px;
  text-align: center;
}

.carousel-slide img {
  width: 80px;
  height: 80px;
  border-radius: 50%;
  object-fit: cover;
  margin-bottom: 16px;
}

.carousel-slide h4 {
  margin: 0 0 8px;
}

.carousel-slide p {
  color: #666;
  font-size: 14px;
  line-height: 1.6;
}

.carousel-prev,
.carousel-next {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  width: 40px;
  height: 40px;
  background: white;
  border: none;
  border-radius: 50%;
  box-shadow: 0 2px 8px rgba(0,0,0,0.15);
  cursor: pointer;
  font-size: 24px;
  z-index: 10;
}

.carousel-prev { left: 0; }
.carousel-next { right: 0; }

.carousel-dots {
  display: flex;
  justify-content: center;
  gap: 8px;
  margin-top: 20px;
}
```
