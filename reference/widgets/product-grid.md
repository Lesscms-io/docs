# Product Grid Widget

> **LessCommerce Widget** — This widget requires an active [LessCommerce](https://lesscommerce.io) account connected to your LessCMS project.

Displays products in a responsive grid layout. Supports filtering by source (latest, category, or featured) with configurable columns and display options.

## Widget Type

```
product-grid
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"product-grid"` |
| `uuid` | string | Unique widget identifier |
| `widget.config` | object | Config element group |
| `widget.config.source` | string | Product source: `"latest"`, `"category"`, `"featured"`, or `"manual"` |
| `widget.config.category_source` | string | Where the category slug comes from (when `source="category"`): `"static"` (hard-coded) or `"url"` (read from URL segment) |
| `widget.config.category_slug` | string | Category slug to filter by (used when `source="category"` and `category_source="static"`) |
| `widget.config.category_url_segment` | number | URL path segment (0-indexed) holding the category slug (used when `source="category"` and `category_source="url"`). E.g. for `/kategoria/elektronika` set to `1`. |
| `widget.config.product_slugs` | string | Newline- or comma-separated list of product slugs (used when `source` is `"manual"`). Order is preserved; missing products are silently dropped. |
| `widget.config.limit` | number | Maximum number of products to display (ignored when `source` is `"manual"`) |
| `widget.config.columns` | number | Number of columns on desktop |
| `widget.config.columns_tablet` | number | Number of columns on tablet |
| `widget.config.columns_mobile` | number | Number of columns on mobile |
| `widget.config.show_price` | boolean | Whether to display product prices |
| `widget.config.show_category` | boolean | Whether to display product category labels |
| `widget.heading` | object | Heading element group |
| `widget.heading.text` | object | Multilingual heading text |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "product-grid",
  "uuid": "pg-101",
  "widget": {
    "config": {
      "source": "latest",
      "category_source": "static",
      "category_slug": "",
      "category_url_segment": 1,
      "product_slugs": "",
      "limit": 8,
      "columns": 4,
      "columns_tablet": 2,
      "columns_mobile": 1,
      "show_price": true,
      "show_category": true
    },
    "heading": {
      "text": {
        "en": "Our Products",
        "pl": "Nasze produkty"
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
| `manual` | Hand-picked list of products by slug (use `product_slugs`, one per line). Order is preserved. Useful for landing pages with curated product selections. |

## Usage Example

```javascript
function renderProductGrid(widget, products, language) {
  const { config, heading } = widget.widget;
  const title = heading?.text?.[language] || heading?.text?.en || '';

  let html = '';
  if (title) {
    html += `<h2>${title}</h2>`;
  }

  html += `<div class="product-grid" style="display: grid; grid-template-columns: repeat(${config.columns}, 1fr); gap: 20px;">`;

  for (const product of products.slice(0, config.limit)) {
    html += `<div class="product-card">`;
    html += `<img src="${product.image}" alt="${product.name}" />`;
    html += `<h3>${product.name}</h3>`;
    if (config.show_category && product.category) {
      html += `<span class="category">${product.category}</span>`;
    }
    if (config.show_price) {
      html += `<span class="price">${product.price}</span>`;
    }
    html += `</div>`;
  }

  html += `</div>`;
  return html;
}
```
