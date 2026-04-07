# Product Detail Widget

> **LessCommerce Widget** — This widget requires an active [LessCommerce](https://lesscommerce.io) account connected to your LessCMS project.

A full product detail page widget with gallery, name, price, variants, description, specifications, and add-to-cart button. Typically placed on the auto-generated system `/product/:slug` page where the product slug is read from the URL.

## Widget Type

```
product-detail
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"product-detail"` |
| `uuid` | string | Unique widget identifier |
| `widget.config` | object | Config element group |
| `widget.config.show_gallery` | boolean | Display image gallery |
| `widget.config.show_variants` | boolean | Display variant selector (color, size, etc.) |
| `widget.config.show_description` | boolean | Display product description section |
| `widget.config.show_specifications` | boolean | Display product specifications table |
| `widget.config.slug_source` | string | Where product slug comes from: `"url"` or `"static"` |
| `widget.config.slug_url_segment` | number | Which URL segment holds the slug (when `slug_source="url"`) |
| `widget.config.slug` | string | Hard-coded product slug (when `slug_source="static"`). Use this for landing pages dedicated to a specific product. |
| `widget.add_to_cart_button` | object | Add to cart button element group |
| `widget.add_to_cart_button.text` | string \| object | Add to cart button label (multilingual) |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "product-detail",
  "uuid": "product-detail-001",
  "widget": {
    "config": {
      "show_gallery": true,
      "show_variants": true,
      "show_description": true,
      "show_specifications": true,
      "slug_source": "url",
      "slug_url_segment": 2,
      "slug": ""
    },
    "add_to_cart_button": { "text": { "pl": "Dodaj do koszyka", "en": "Add to cart" } }
  },
  "settings": {
    "padding_top": 30,
    "padding_bottom": 30,
    "responsive": { "tablet": {}, "mobile": {} }
  }
}
```

## Data Source

The widget resolves the product slug from the URL (or static config) and fetches the product from the LessCommerce API:

- `GET /api/products/by-slug/{slug}` — fetch product details by slug
- `POST /api/cart/items` — add the selected variant + quantity to the cart

## URL Slug Resolution

With `slug_source="url"` and `slug_url_segment=2`, for a URL like `/product/blue-shirt`, the resolved slug is `"blue-shirt"` (0-indexed segment 1, so segment 2 is the second path part).

## Usage Example

```javascript
async function renderProductDetail(widget, urlPath) {
  const { config, add_to_cart_button } = widget.widget;
  const slug = urlPath.split('/').filter(Boolean)[config.slug_url_segment - 1];
  const product = await fetch(`/api/products/by-slug/${slug}`).then(r => r.json());

  return `
    <article>
      <h1>${product.name}</h1>
      <div class="price">${product.price}</div>
      ${config.show_gallery ? renderGallery(product.images) : ''}
      ${config.show_description ? `<p>${product.description}</p>` : ''}
      <button data-add-to-cart="${product.id}">${add_to_cart_button.text.pl}</button>
    </article>
  `;
}
```
