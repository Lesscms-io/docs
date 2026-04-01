# Product Carousel Widget

> **LessCommerce Widget** — This widget requires an active [LessCommerce](https://lesscommerce.io) account connected to your LessCMS project.

Displays products in a horizontal carousel/slider. Supports filtering by source (latest, category, or featured) with configurable slide count and autoplay.

## Widget Type

```
product-carousel
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"product-carousel"` |
| `uuid` | string | Unique widget identifier |
| `widget.config` | object | Config element group |
| `widget.config.source` | string | Product source: `"latest"`, `"category"`, or `"featured"` |
| `widget.config.category_slug` | string | Category slug to filter by (used when `source` is `"category"`) |
| `widget.config.limit` | number | Maximum number of products to load |
| `widget.config.visible_items` | number | Number of product cards visible at once |
| `widget.config.autoplay` | boolean | Whether the carousel auto-advances |
| `widget.config.show_price` | boolean | Whether to display product prices |
| `widget.heading` | object | Heading element group |
| `widget.heading.text` | object | Multilingual heading text |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "product-carousel",
  "uuid": "pc-404",
  "widget": {
    "config": {
      "source": "latest",
      "category_slug": "",
      "limit": 8,
      "visible_items": 4,
      "autoplay": false,
      "show_price": true
    },
    "heading": {
      "text": {
        "en": "New Arrivals",
        "pl": "Nowości"
      }
    }
  },
  "settings": {
    "padding_top": 20,
    "padding_bottom": 20,
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Product Sources

| Value | Description |
|-------|-------------|
| `latest` | Most recently added products |
| `category` | Products from a specific category (use `category_slug` to specify) |
| `featured` | Products marked as featured in LessCommerce |

## Usage Example

```javascript
function renderProductCarousel(widget, products, language) {
  const { config, heading } = widget.widget;
  const title = heading?.text?.[language] || heading?.text?.en || '';

  let html = '';
  if (title) {
    html += `<h2>${title}</h2>`;
  }

  html += `<div class="product-carousel" data-visible="${config.visible_items}" data-autoplay="${config.autoplay}">`;
  html += `<div class="carousel-track">`;

  for (const product of products.slice(0, config.limit)) {
    html += `<div class="carousel-slide">`;
    html += `<img src="${product.image}" alt="${product.name}" />`;
    html += `<h3>${product.name}</h3>`;
    if (config.show_price) {
      html += `<span class="price">${product.price}</span>`;
    }
    html += `</div>`;
  }

  html += `</div>`;
  html += `<button class="carousel-prev"><i class="fa-solid fa-chevron-left"></i></button>`;
  html += `<button class="carousel-next"><i class="fa-solid fa-chevron-right"></i></button>`;
  html += `</div>`;
  return html;
}
```
