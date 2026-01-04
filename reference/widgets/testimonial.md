# Testimonial Widget

A customer testimonial or review card with quote, author info, and optional rating.

## Widget Type

```
testimonial
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"testimonial"` |
| `uuid` | string | Unique widget identifier |
| `config` | object | Widget configuration |
| `config.rating` | number | Star rating (1-5, optional) |
| `config.image` | object | Author avatar image (optional) |
| `content` | object | Widget content |
| `content.quote` | string | Testimonial text (localized) |
| `content.author` | string | Author name (localized) |
| `content.position` | string | Author position/title (localized) |
| `settings` | object | Style settings (optional) |

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
  "config": {
    "rating": 5,
    "image": {
      "url": "https://cdn.example.com/avatars/john.jpg",
      "alt": { "en": "John Smith" }
    }
  },
  "content": {
    "quote": "Working with this team has been an absolute pleasure. They delivered our project on time and exceeded all our expectations.",
    "author": "John Smith",
    "position": "CEO, Tech Company"
  },
  "settings": {
    "paddingTop": 32,
    "paddingBottom": 32,
    "backgroundColor": "#F8F9FA",
    "borderRadius": 12
  }
}
```

## Example Response (Without Image)

```json
{
  "widget_type": "testimonial",
  "uuid": "testimonial-456",
  "config": {
    "rating": 4,
    "image": null
  },
  "content": {
    "quote": "Excellent service and great results. Would highly recommend!",
    "author": "Jane Doe",
    "position": "Marketing Director"
  },
  "settings": {}
}
```

## Usage Example

```javascript
// Render testimonial widget
function renderTestimonial(widget, language) {
  const { rating, image } = widget.config;
  const { quote, author, position } = widget.content;
  const settings = widget.settings || {};

  const imageUrl = image?.url || '';
  const imageAlt = image?.alt?.[language] || image?.alt?.en || author;

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
        "${quote}"
      </blockquote>
      <div class="testimonial-author">
        ${imageUrl ? `
          <img src="${imageUrl}" alt="${imageAlt}" class="testimonial-avatar">
        ` : ''}
        <div class="testimonial-author-info">
          <strong class="testimonial-name">${author}</strong>
          ${position ? `<span class="testimonial-position">${position}</span>` : ''}
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
