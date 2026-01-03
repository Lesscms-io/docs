# Testimonial Widget

A customer testimonial or review card with quote, author info, and optional rating.

## Widget Type

```
testimonial
```

## Data Properties

| Property | Type | Global | Description |
|----------|------|--------|-------------|
| `quote` | object | No | Testimonial text (multilingual) |
| `author` | object | No | Author name (multilingual) |
| `position` | object | No | Author position/title (multilingual) |
| `image` | object | Yes | Author avatar image |
| `rating` | number | Yes | Star rating (1-5) |

### Image Object Structure

| Property | Type | Description |
|----------|------|-------------|
| `url` | string | Avatar image URL |
| `alt` | object | Alt text (multilingual) |

## Example Response

```json
{
  "widget_type": "testimonial",
  "uuid": "testimonial-123",
  "data": {
    "quote": {
      "en": "Working with this team has been an absolute pleasure. They delivered our project on time and exceeded all our expectations.",
      "pl": "Praca z tym zespołem była absolutną przyjemnością. Dostarczyli nasz projekt na czas i przekroczyli wszystkie nasze oczekiwania."
    },
    "author": {
      "en": "John Smith",
      "pl": "John Smith"
    },
    "position": {
      "en": "CEO, Tech Company",
      "pl": "CEO, Tech Company"
    },
    "image": {
      "url": "https://cdn.example.com/avatars/john.jpg",
      "alt": { "en": "John Smith" }
    },
    "rating": 5
  },
  "settings": {
    "paddingTop": 32,
    "paddingBottom": 32,
    "backgroundColor": "#F8F9FA",
    "borderRadius": 12
  }
}
```

## Usage Example

```javascript
// Render testimonial widget
function renderTestimonial(widget, language) {
  const { quote, author, position, image, rating } = widget.data;
  const settings = widget.settings || {};

  const quoteText = quote?.[language] || quote?.en || '';
  const authorText = author?.[language] || author?.en || '';
  const positionText = position?.[language] || position?.en || '';
  const imageUrl = image?.url || '';
  const imageAlt = image?.alt?.[language] || image?.alt?.en || authorText;

  // Generate stars
  let starsHtml = '';
  if (rating) {
    for (let i = 1; i <= 5; i++) {
      if (i <= rating) {
        starsHtml += '<i class="fa-solid fa-star"></i>';
      } else {
        starsHtml += '<i class="fa-regular fa-star"></i>';
      }
    }
  }

  return `
    <div class="testimonial-widget">
      ${starsHtml ? `<div class="testimonial-rating">${starsHtml}</div>` : ''}
      <blockquote class="testimonial-quote">
        "${quoteText}"
      </blockquote>
      <div class="testimonial-author">
        ${imageUrl ? `
          <img src="${imageUrl}" alt="${imageAlt}" class="testimonial-avatar">
        ` : ''}
        <div class="testimonial-author-info">
          <strong class="testimonial-name">${authorText}</strong>
          ${positionText ? `<span class="testimonial-position">${positionText}</span>` : ''}
        </div>
      </div>
    </div>
  `;
}
```

## CSS Example

```css
.testimonial-widget {
  text-align: center;
  padding: 32px;
  background: #F8F9FA;
  border-radius: 12px;
}

.testimonial-rating {
  margin-bottom: 16px;
  color: #FFD700;
  font-size: 18px;
}

.testimonial-rating i {
  margin: 0 2px;
}

.testimonial-quote {
  font-size: 18px;
  font-style: italic;
  color: #333;
  line-height: 1.6;
  margin: 0 0 24px;
  padding: 0;
  border: none;
}

.testimonial-author {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 12px;
}

.testimonial-avatar {
  width: 56px;
  height: 56px;
  border-radius: 50%;
  object-fit: cover;
}

.testimonial-author-info {
  text-align: left;
}

.testimonial-name {
  display: block;
  font-size: 16px;
  color: #333;
}

.testimonial-position {
  display: block;
  font-size: 14px;
  color: #666;
}
```

## Testimonial Carousel

For multiple testimonials, consider using a carousel:

```javascript
function renderTestimonialCarousel(testimonials, language) {
  const slides = testimonials
    .map(widget => renderTestimonial(widget, language))
    .join('');

  return `
    <div class="testimonial-carousel">
      <div class="carousel-track">${slides}</div>
      <div class="carousel-controls">
        <button class="prev">←</button>
        <button class="next">→</button>
      </div>
    </div>
  `;
}
```
