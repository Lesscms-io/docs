# Google Reviews Widget

Displays Google reviews fetched from the Google Places API. Reviews are cached in widget data and can be auto-refreshed. Supports grid, carousel, and list layouts.

## Widget Type

```
google-reviews
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"google-reviews"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget data (element groups) |
| `widget.config` | object | Configuration |
| `widget.config.place_id` | string | Google Place ID (used for fetching, not sensitive) |
| `widget.config.layout` | string | `"grid"`, `"carousel"`, or `"list"` (default: `"grid"`) |
| `widget.config.columns` | number | Grid columns (default: 3) |
| `widget.config.sort_by` | string | `"most_relevant"` or `"newest"` |
| `widget.config.show_rating_summary` | boolean | Show rating summary header (default: true) |
| `widget.config.max_reviews` | number | Maximum reviews to display (1-5) |
| `widget.config.min_rating` | number | Minimum star rating filter (0-5) |
| `widget.place` | object | Place information |
| `widget.place.name` | string | Business name |
| `widget.place.address` | string | Business address |
| `widget.place.overall_rating` | number | Average rating (0-5) |
| `widget.place.total_reviews` | number | Total number of reviews |
| `widget.summary` | object | Summary section styling |
| `widget.summary.color` | string\|null | Summary text color |
| `widget.summary.color:hover` | string\|null | Summary text hover color |
| `widget.summary.background` | string\|null | Summary background color |
| `widget.summary.background:hover` | string\|null | Summary background hover color |
| `widget.card` | object | Review card styling |
| `widget.card.background` | string\|null | Card background color |
| `widget.card.background:hover` | string\|null | Card background hover color |
| `widget.card.border_color` | string\|null | Card border color |
| `widget.card.border_color:hover` | string\|null | Card border hover color |
| `widget.card.border_radius` | number | Card border radius in px (default: 8) |
| `widget.author` | object | Author text styling |
| `widget.author.color` | string\|null | Author name color |
| `widget.author.color:hover` | string\|null | Author name hover color |
| `widget.rating` | object | Star rating styling |
| `widget.rating.color` | string\|null | Star color |
| `widget.rating.color:hover` | string\|null | Star hover color |
| `widget.review` | object | Review text styling |
| `widget.review.color` | string\|null | Review text color |
| `widget.review.color:hover` | string\|null | Review text hover color |
| `widget.review.max_chars` | number | Max characters before truncation (default: 200) |
| `widget.reviews` | array | Array of review objects |
| `widget.reviews[].author_name` | string | Reviewer name |
| `widget.reviews[].author_photo_url` | string | Reviewer profile photo URL |
| `widget.reviews[].author_url` | string | Reviewer Google Maps profile URL |
| `widget.reviews[].rating` | number | Star rating (1-5) |
| `widget.reviews[].text` | string | Review text content |
| `widget.reviews[].time` | string | ISO 8601 publish timestamp |
| `widget.reviews[].relative_time` | string | Human-readable relative time (e.g., "2 miesiące temu") |
| `widget.last_fetched_at` | string\|null | ISO 8601 timestamp of when reviews were last fetched |
| `settings` | object | Common style settings (padding, margin, background, etc.) |

## Example Response

```json
{
  "widget_type": "google-reviews",
  "uuid": "gr-001",
  "widget": {
    "config": {
      "place_id": "ChIJN1t_tDeuEmsRUsoyG83frY4",
      "layout": "grid",
      "columns": 3,
      "sort_by": "most_relevant",
      "show_rating_summary": true,
      "max_reviews": 5,
      "min_rating": 0
    },
    "place": {
      "name": "Restauracja Przykładowa",
      "address": "ul. Marszałkowska 1, 00-001 Warszawa",
      "overall_rating": 4.6,
      "total_reviews": 342
    },
    "summary": {
      "color": "var:dark",
      "color:hover": null,
      "background": null,
      "background:hover": null
    },
    "card": {
      "background": "var:white",
      "background:hover": null,
      "border_color": "var:border",
      "border_color:hover": "var:primary",
      "border_radius": 8
    },
    "author": {
      "color": "var:dark",
      "color:hover": null
    },
    "rating": {
      "color": null,
      "color:hover": null
    },
    "review": {
      "color": "var:muted",
      "color:hover": null,
      "max_chars": 200
    },
    "reviews": [
      {
        "author_name": "Jan Kowalski",
        "author_photo_url": "https://lh3.googleusercontent.com/a/...",
        "author_url": "https://www.google.com/maps/contrib/...",
        "rating": 5,
        "text": "Świetna obsługa i pyszne jedzenie. Polecam wszystkim!",
        "time": "2026-03-15T10:30:00Z",
        "relative_time": "2 tygodnie temu"
      },
      {
        "author_name": "Anna Nowak",
        "author_photo_url": "https://lh3.googleusercontent.com/a/...",
        "author_url": "https://www.google.com/maps/contrib/...",
        "rating": 4,
        "text": "Bardzo dobre miejsce, klimatyczny wystrój. Jedyny minus to czas oczekiwania na zamówienie.",
        "time": "2026-03-10T14:20:00Z",
        "relative_time": "3 tygodnie temu"
      }
    ],
    "last_fetched_at": "2026-04-01T05:00:00Z"
  },
  "settings": {
    "padding_top": 20,
    "padding_bottom": 20,
    "transition_duration": 200
  }
}
```

## Important Notes

- **Google Places API returns a maximum of 5 reviews** per request. This is a hard limit.
- Reviews are fetched from Google and stored in widget data — the Public API just reads them from MongoDB.
- `author_photo_url` may require `referrerpolicy="no-referrer"` on `<img>` tags to load correctly.
- **Google attribution is required** by Google's Terms of Service when displaying reviews.
- Color values use the `var:colorCode` or `var:colorCode:opacity` format — resolve them to CSS custom properties.

## Usage Example

```javascript
function renderGoogleReviews(widget, language) {
  const { config, place, reviews } = widget.widget;

  // Filter by min rating
  let filtered = reviews;
  if (config.min_rating > 0) {
    filtered = reviews.filter(r => r.rating >= config.min_rating);
  }
  filtered = filtered.slice(0, config.max_reviews);

  // Render summary
  if (config.show_rating_summary && place.name) {
    console.log(`${place.name} — ${place.overall_rating}/5 (${place.total_reviews} reviews)`);
  }

  // Render reviews
  filtered.forEach(review => {
    console.log(`${review.author_name} — ${'★'.repeat(review.rating)}${'☆'.repeat(5 - review.rating)}`);
    const text = review.text.length > config.max_reviews
      ? review.text.substring(0, widget.widget.review.max_chars) + '...'
      : review.text;
    console.log(text);
  });
}
```
