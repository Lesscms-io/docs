# Related Products Widget

Renders products related to the one currently on screen. Meant for product
detail pages, where it sits under the product description or above the footer.

Since 2026-08-16 the widget can resolve its list from the shop's **related
product rules** (LessCommerce → Shop settings → Related products) instead of
its two original hard-coded modes. Rules support manual picks, variant family,
shared category, product template, matching attribute, name similarity and
price proximity, evaluated in order until the limit is filled.

## Widget Type

```
related-products
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"related-products"` |
| `uuid` | string | Unique widget identifier |
| `widget.config` | object | Config element group |
| `widget.config.basis` | string | How the list is resolved: `"rules"` (shop rule sets — recommended), `"category"` (same category as the current product), `"template"` (same product template). Defaults to `"rules"` for new widgets; widgets saved before this change keep `"category"`. |
| `widget.config.related_set` | string | Which rule set to run when `basis="rules"`. Empty = first enabled set. |
| `widget.config.slug_source` | string | Where the current product's slug comes from: `"url"` or `"static"` |
| `widget.config.slug_url_segment` | number | 1-indexed URL segment holding the slug when `slug_source="url"` |
| `widget.config.slug` | string | Hard-coded product slug when `slug_source="static"` |
| `widget.config.limit` | number | Maximum number of products to display |
| `widget.config.columns` | number | Columns on desktop |
| `widget.config.columns_tablet` | number | Columns on tablet |
| `widget.config.columns_mobile` | number | Columns on mobile |
| `widget.config.show_price` | boolean | Whether to display prices |
| `widget.config.show_category` | boolean | Whether to display category labels |
| `widget.heading` | object | Heading element group |
| `widget.heading.text` | string | Optional section heading (multilingual) |

## Example Response

```json
{
  "widget_type": "related-products",
  "uuid": "b1f2c3d4-5e6f-4a7b-8c9d-0e1f2a3b4c5d",
  "widget": {
    "config": {
      "basis": "rules",
      "related_set": "similar",
      "slug_source": "url",
      "slug_url_segment": 1,
      "slug": "",
      "limit": 4,
      "columns": 4,
      "columns_tablet": 2,
      "columns_mobile": 2,
      "show_price": true,
      "show_category": false
    },
    "heading": {
      "text": { "pl": "Podobne produkty", "en": "You may also like" }
    }
  },
  "settings": {
    "padding_top": 20,
    "padding_bottom": 20
  }
}
```

## How `basis="rules"` resolves

1. The widget resolves the current product (injected product from the renderer,
   then `slug` route param, then the configured URL segment).
2. It calls the storefront endpoint `GET /products/{slug}/related?set=&limit=`.
3. The storefront runs the shop's rule set over its cached product list — no
   Core API round-trip, so the call stays inside one Redis read.
4. If the shop has no rules configured, or the rules return nothing, the widget
   falls back to the legacy same-category behaviour so existing pages never
   render an empty section after upgrading.

## Notes

- With `basis="category"` or `"template"` the widget keeps its original
  behaviour and ignores shop rules entirely — an explicit choice is respected.
- No product in context renders an empty widget rather than a grid of unrelated
  products.
- The same rule engine is available on `product-grid` and `product-carousel`
  via `source="related"` when you need the full layout options of those widgets.

## Related

- [Product Grid](product-grid.md)
- [Product Carousel](product-carousel.md)
- [Product Detail](product-detail.md)
