# Star Rating Widget

A star rating display widget for showing ratings.

## Widget Type

```
star-rating
```

## Data Properties

| Property | Type | Global | Description |
|----------|------|--------|-------------|
| `rating` | number | Yes | Current rating value (e.g., 4.5) |
| `max_stars` | number | Yes | Maximum number of stars (default: 5) |
| `color` | string | Yes | Star color (hex code) |

## Example Response

```json
{
  "widget_type": "star-rating",
  "uuid": "rating-123",
  "data": {
    "rating": 4.5,
    "max_stars": 5,
    "color": "#FFD700"
  },
  "settings": {
    "textAlign": "center"
  }
}
```

## Usage Example

```javascript
// Render star rating widget
function renderStarRating(widget) {
  const { rating, max_stars, color } = widget.data;
  const maxStars = max_stars || 5;

  let starsHtml = '';

  for (let i = 1; i <= maxStars; i++) {
    if (i <= Math.floor(rating)) {
      // Full star
      starsHtml += `<i class="fa-solid fa-star" style="color: ${color};"></i>`;
    } else if (i - 0.5 <= rating) {
      // Half star
      starsHtml += `<i class="fa-solid fa-star-half-stroke" style="color: ${color};"></i>`;
    } else {
      // Empty star
      starsHtml += `<i class="fa-regular fa-star" style="color: ${color};"></i>`;
    }
  }

  return `
    <div class="star-rating" aria-label="Rating: ${rating} out of ${maxStars}">
      ${starsHtml}
    </div>
  `;
}
```

## Accessibility

For better accessibility, include the numeric rating as text or aria-label:

```javascript
function renderAccessibleStarRating(widget) {
  const { rating, max_stars } = widget.data;

  return `
    <div class="star-rating" role="img" aria-label="Rating: ${rating} out of ${max_stars || 5} stars">
      ${renderStars(widget)}
      <span class="sr-only">${rating} / ${max_stars || 5}</span>
    </div>
  `;
}
```
