# Product Grid Widget

> **LessCommerce Widget** — This widget requires an active [LessCommerce](https://lesscommerce.io) account connected to your LessCMS project.

Displays products in a responsive grid layout. Supports filtering by source (latest, category, featured, manual, or search results) with configurable columns and display options.

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
| `widget.config.source` | string | Product source: `"latest"`, `"category"`, `"featured"`, `"manual"`, `"search"`, or `"related"` |
| — related mode | | With `source="related"` the grid renders products resolved by the shop's related-product rules (Shop settings → Related products). It reads the current product from the injected product (SSR), then the route `slug` param, then the URL segment given by `related_slug_url_segment`. No product in context renders an empty grid rather than falling back to unrelated products. |
| `widget.config.related_set` | string | Which rule set to run (used when `source="related"`). Empty = first enabled set. |
| `widget.config.related_slug_url_segment` | number | 1-indexed URL segment holding the product slug, used only when the product is not injected and the route has no `slug` param. Defaults to `1`. |
| `widget.config.category_source` | string | Where the category slug comes from (when `source="category"`): `"static"` (hard-coded) or `"url"` (read from URL segment) |
| `widget.config.category_slug` | string | Category slug to filter by (used when `source="category"` and `category_source="static"`) |
| `widget.config.category_url_segment` | number | URL path segment (0-indexed) holding the category slug (used when `source="category"` and `category_source="url"`). E.g. for `/kategoria/elektronika` set to `1`. |
| `widget.config.product_slugs` | string | Newline- or comma-separated list of product slugs (used when `source` is `"manual"`). Order is preserved; missing products are silently dropped. |
| — search mode | | With `source="search"` the grid reads the `?q=` URL parameter (the search-bar widget navigates there) and calls the storefront `/products/search` endpoint. Empty `q` renders a "type a phrase" prompt; no hits render "no results for …". Pagination works via `?page=`. Place the widget on the page the search bar routes to (`commerce.routes.search`, default `/szukaj`). |
| `widget.config.limit` | number | Maximum number of products to display (ignored when `source` is `"manual"`) |
| `widget.config.columns` | number | Number of columns on desktop |
| `widget.config.columns_tablet` | number | Number of columns on tablet |
| `widget.config.columns_mobile` | number | Number of columns on mobile |
| `widget.config.gap` | number | Gap between tiles in pixels (default: `12`) |
| `widget.config.show_price` | boolean | Whether to display product prices |
| `widget.config.show_category` | boolean | Whether to display product category labels |
| `widget.config.show_discount_badge` | boolean | Fallback auto discount badge (`-X%`) shown when the product has no marketing labels. Set `false` to hide. Defaults to `true`. |
| `widget.config.field_image` | string | Path to the product image field. Defaults to `"image"`. Use `attributes.<code>` to read from a template attribute. |
| `widget.config.field_name` | string | Path to the product name field. Defaults to `"name"`. |
| `widget.config.field_price` | string | Path to the product price field. Defaults to `"price"`. |
| `widget.config.field_category` | string | Path to the product category label. Defaults to `"category.name"`. |
| `widget.config.field_description` | string | Optional path to a description/subtitle field shown under the product name. Empty = no description. |
| `widget.heading` | object | Heading element group |
| `widget.heading.text` | object | Multilingual heading text |
| `widget.subtitle` | object | Subtitle element group (shown under the heading) |
| `widget.subtitle.text` | object | Multilingual subtitle text |
| `widget.see_all` | object | "See all" link shown in the top-right of the header |
| `widget.see_all.text` | object | Multilingual link text (e.g. `"Zobacz wszystkie"`). Empty = link hidden. |
| `widget.see_all.url` | string | Link target URL. Empty = link hidden. |
| `widget.all_tile` | object | Optional "all products" tile appended as the last grid cell |
| `widget.all_tile.enabled` | boolean | Render the tile at the end of the grid (default `false`) |
| `widget.all_tile.text` | object | Multilingual tile label. Empty = "Wszystkie produkty"/"All products". |
| `widget.all_tile.url` | string | Tile target URL. Empty = falls back to `see_all.url`; no URL at all hides the tile. |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Marketing Labels (Badges)

Each product in the response includes a `marketing_labels[]` array populated from the LessCommerce Marketing Labels module (both manual assignments and rule-based matches). Render these as badges in place of the auto discount badge.

Each label has:

| Field | Type | Description |
|-------|------|-------------|
| `uuid` | string | Label identifier |
| `code` | string | Label code (e.g. `"promo"`, `"new"`) |
| `text` | string | Display text (e.g. `"PROMOCJA"`, `"NOWOŚĆ"`) |
| `text_translation` | object | Multilingual overrides: `{ en: "SALE", pl: "PROMOCJA" }` |
| `background_color` | string | Hex/CSS color for the badge background |
| `text_color` | string | Hex/CSS color for the badge text |
| `sort_order` | number | Render order (lower first) |
| `source` | string | `"manual"` or `"rule"` |

When a product has at least one label in `marketing_labels[]`, render all of them (sorted by `sort_order`) and **skip** the auto discount badge. When the array is empty and `show_discount_badge` is `true`, fall back to the auto `-X%` badge computed from `compare_at_price` vs `price`.

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
      "gap": 12,
      "show_price": true,
      "show_category": true,
      "show_discount_badge": true,
      "field_image": "image",
      "field_name": "name",
      "field_price": "price",
      "field_category": "category.name",
      "field_description": ""
    },
    "heading": {
      "text": {
        "en": "Featured products",
        "pl": "Polecane produkty"
      }
    },
    "subtitle": {
      "text": {
        "en": "Hand-picked products loved by our customers",
        "pl": "Ręcznie wybrane produkty, które pokochali nasi klienci"
      }
    },
    "see_all": {
      "text": {
        "en": "See all",
        "pl": "Zobacz wszystkie"
      },
      "url": "/products"
    },
    "all_tile": {
      "enabled": true,
      "text": {
        "en": "All products",
        "pl": "Wszystkie produkty"
      },
      "url": "/products"
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
  const { config, heading, subtitle, see_all } = widget.widget;
  const title = heading?.text?.[language] || heading?.text?.en || '';
  const sub = subtitle?.text?.[language] || subtitle?.text?.en || '';
  const seeAllText = see_all?.text?.[language] || see_all?.text?.en || '';
  const showSeeAll = seeAllText && see_all?.url;

  let html = '';
  if (title || sub || showSeeAll) {
    html += `<div class="pg-header">`;
    html += `<div>`;
    if (title) html += `<h2>${title}</h2>`;
    if (sub) html += `<p>${sub}</p>`;
    html += `</div>`;
    if (showSeeAll) {
      html += `<a href="${see_all.url}" class="pg-see-all">${seeAllText}</a>`;
    }
    html += `</div>`;
  }

  html += `<div class="product-grid" style="display: grid; grid-template-columns: repeat(${config.columns}, 1fr); gap: 20px;">`;

  for (const product of products.slice(0, config.limit)) {
    html += `<div class="product-card">`;
    html += `<div class="product-card__image-wrap">`;
    html += `<img src="${product.image}" alt="${product.name}" />`;

    // Render marketing labels from LessCommerce, or fall back to auto discount badge
    const labels = product.marketing_labels || [];
    if (labels.length > 0) {
      html += `<div class="product-card__labels">`;
      for (const label of labels) {
        const text = label.text_translation?.[language] || label.text;
        const style = `background:${label.background_color};color:${label.text_color}`;
        html += `<span class="product-card__label" style="${style}">${text}</span>`;
      }
      html += `</div>`;
    } else if (config.show_discount_badge && product.compare_at_price > product.price) {
      const pct = Math.round((1 - product.price / product.compare_at_price) * 100);
      html += `<span class="product-card__badge-discount">-${pct}%</span>`;
    }
    html += `</div>`;

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
