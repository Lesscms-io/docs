# Category Tree

Expandable tree navigation of product categories from LessCommerce. Sidebar-style
widget: parent categories toggle with a chevron, the category matching the current
URL is highlighted and its ancestors auto-expand.

## Widget Type

`category-tree`

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget.config` | object | Widget configuration |
| `widget.config.expand_mode` | string | Initial expansion: `active` (default — expand the path to the category matching the current URL), `all`, `none` |
| `widget.config.show_all_link` | boolean | Render an "All products" link above the tree (default `false`) |
| `widget.config.all_link_url` | string | Target of the "All products" link (default `/produkty`) |
| `widget.heading` | object | Heading element group |
| `widget.heading.text` | object | Multilingual heading text. Empty = heading hidden. |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "category-tree",
  "uuid": "a1b2c3d4-...",
  "widget": {
    "config": {
      "expand_mode": "active",
      "show_all_link": true,
      "all_link_url": "/produkty"
    },
    "heading": {
      "text": { "pl": "Kategorie", "en": "Categories" }
    }
  },
  "settings": {
    "padding_top": 20,
    "padding_bottom": 20
  }
}
```

## Data Source

Categories come from the LessCommerce storefront API (`GET /categories`), which
returns the full tree (each node carries `children[]`). Category links use the
project's commerce route config (`commerce.routes.category`, default
`/kategoria/:slug`).

## Usage Notes

- The active category is derived from the current URL by matching the category
  route prefix, so the same widget instance works on every category page.
- Expansion state is client-side only; SSR renders the tree according to
  `expand_mode` with no URL context (`active` behaves like `none` server-side,
  then hydrates on the client).
