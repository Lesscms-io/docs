# Image Carousel Widget

A slideshow carousel for multiple images with autoplay and navigation options.

## Widget Type

```
image-carousel
```

## Data Properties

| Property | Type | Global | Description |
|----------|------|--------|-------------|
| `images` | array | Yes | Array of image objects |
| `autoplay` | boolean | Yes | Enable automatic slide advancement |
| `interval` | number | Yes | Autoplay interval in milliseconds |
| `show_dots` | boolean | Yes | Show navigation dots |
| `show_arrows` | boolean | Yes | Show prev/next arrows |

### Image Object Structure

Each image in the `images` array contains:

| Property | Type | Description |
|----------|------|-------------|
| `url` | string | Image URL |
| `alt` | object | Alt text (multilingual) |
| `title` | object | Title/caption (multilingual) |

## Example Response

```json
{
  "widget_type": "image-carousel",
  "uuid": "carousel-123",
  "data": {
    "images": [
      {
        "url": "https://cdn.example.com/slide1.jpg",
        "alt": { "en": "Slide 1", "pl": "Slajd 1" },
        "title": { "en": "Welcome", "pl": "Witamy" }
      },
      {
        "url": "https://cdn.example.com/slide2.jpg",
        "alt": { "en": "Slide 2", "pl": "Slajd 2" },
        "title": { "en": "Our Services", "pl": "Nasze usługi" }
      },
      {
        "url": "https://cdn.example.com/slide3.jpg",
        "alt": { "en": "Slide 3", "pl": "Slajd 3" },
        "title": { "en": "Contact Us", "pl": "Kontakt" }
      }
    ],
    "autoplay": true,
    "interval": 5000,
    "show_dots": true,
    "show_arrows": true
  },
  "settings": {
    "height": 500,
    "borderRadius": 8
  }
}
```

## Usage Example

```javascript
// Basic carousel structure (use with Swiper, Glide, or similar library)
function renderImageCarousel(widget, language) {
  const { images, autoplay, interval, show_dots, show_arrows } = widget.data;
  const settings = widget.settings || {};

  if (!images || images.length === 0) {
    return '';
  }

  const slides = images.map((img, index) => {
    const alt = img.alt?.[language] || img.alt?.en || `Slide ${index + 1}`;
    const title = img.title?.[language] || img.title?.en || '';

    return `
      <div class="carousel-slide">
        <img src="${img.url}" alt="${alt}" loading="${index === 0 ? 'eager' : 'lazy'}">
        ${title ? `<div class="carousel-caption">${title}</div>` : ''}
      </div>
    `;
  }).join('');

  return `
    <div class="image-carousel"
      data-autoplay="${autoplay}"
      data-interval="${interval || 5000}"
      style="height: ${settings.height || 400}px;"
    >
      <div class="carousel-track">${slides}</div>
      ${show_arrows ? `
        <button class="carousel-prev" aria-label="Previous">‹</button>
        <button class="carousel-next" aria-label="Next">›</button>
      ` : ''}
      ${show_dots ? `
        <div class="carousel-dots">
          ${images.map((_, i) => `
            <button class="dot ${i === 0 ? 'active' : ''}" data-index="${i}"></button>
          `).join('')}
        </div>
      ` : ''}
    </div>
  `;
}
```

## Swiper.js Integration

```javascript
import Swiper from 'swiper';
import { Navigation, Pagination, Autoplay } from 'swiper/modules';

function initCarousel(widget) {
  const { autoplay, interval, show_dots, show_arrows } = widget.data;

  return new Swiper(`#carousel-${widget.uuid}`, {
    modules: [Navigation, Pagination, Autoplay],
    loop: true,
    autoplay: autoplay ? {
      delay: interval || 5000,
      disableOnInteraction: false
    } : false,
    navigation: show_arrows ? {
      nextEl: '.swiper-button-next',
      prevEl: '.swiper-button-prev'
    } : false,
    pagination: show_dots ? {
      el: '.swiper-pagination',
      clickable: true
    } : false
  });
}
```

## CSS Example

```css
.image-carousel {
  position: relative;
  overflow: hidden;
}

.carousel-track {
  display: flex;
  transition: transform 0.5s ease;
}

.carousel-slide {
  flex: 0 0 100%;
}

.carousel-slide img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.carousel-caption {
  position: absolute;
  bottom: 20px;
  left: 20px;
  right: 20px;
  color: white;
  text-shadow: 0 2px 4px rgba(0,0,0,0.5);
}

.carousel-prev,
.carousel-next {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(0,0,0,0.5);
  color: white;
  border: none;
  padding: 10px 15px;
  cursor: pointer;
}

.carousel-prev { left: 10px; }
.carousel-next { right: 10px; }

.carousel-dots {
  position: absolute;
  bottom: 15px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  gap: 8px;
}

.dot {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background: rgba(255,255,255,0.5);
  border: none;
  cursor: pointer;
}

.dot.active {
  background: white;
}
```
