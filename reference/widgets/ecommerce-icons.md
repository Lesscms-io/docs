# Ecommerce Icons Widget

> **LessCommerce Widget** — This widget requires an active [LessCommerce](https://lesscommerce.io) account connected to your LessCMS project.

A horizontal row of icon triggers typically placed in a header/toolbar. Each item is one of four built-in types:

- `cart` — icon with live item-count badge; opens a mini-cart dropdown preview
- `search` — icon; opens inline popover with search input + autocomplete (fed by the Storefront API)
- `account` — icon linking to the customer account route
- `custom` — icon + user-provided URL (supports `target="_blank"`)

## Widget Type

```
ecommerce-icons
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"ecommerce-icons"` |
| `uuid` | string | Unique widget identifier |
| `widget.items` | array | Ordered list of icon triggers |
| `widget.items[].type` | string | One of: `"cart"`, `"search"`, `"account"`, `"custom"` |
| `widget.items[].icon` | string\|null | Icon class (FontAwesome / Boxicons / `svg:<markup>`) |
| `widget.items[].url` | string\|null | Override URL (required for `custom`, optional override for `account`) |
| `widget.items[].label` | string\|object\|null | Multilingual aria label |
| `widget.items[].target_blank` | boolean | Open in new tab (only honored for `custom`) |
| `widget.config` | object | Layout configuration |
| `widget.config.size` | number | Icon size in pixels (default: `20`) |
| `widget.config.gap` | number | Horizontal gap between items in pixels (default: `16`) |
| `widget.icon` | object | Shared icon styling |
| `widget.icon.color` | string\|null | Default icon color |
| `widget.icon.color:hover` | string\|null | Icon color on hover |
| `widget.badge` | object | Cart badge styling |
| `widget.badge.background` | string\|null | Cart badge background color |
| `widget.badge.color` | string\|null | Cart badge text color |
| `widget.search` | object | Search popover configuration |
| `widget.search.placeholder` | string\|object\|null | Search input placeholder (multilingual) |
| `widget.search.navigate_url` | string | Target page for search results (default: `"/search"`) |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "ecommerce-icons",
  "uuid": "ei-101",
  "widget": {
    "items": [
      { "type": "search", "icon": "fa-solid fa-magnifying-glass", "url": null, "label": { "pl": "Szukaj" }, "target_blank": false },
      { "type": "account", "icon": "fa-solid fa-user", "url": null, "label": { "pl": "Konto" }, "target_blank": false },
      { "type": "cart", "icon": "fa-solid fa-bag-shopping", "url": null, "label": { "pl": "Koszyk" }, "target_blank": false }
    ],
    "config": { "size": 20, "gap": 16 },
    "icon": { "color": "var:text", "color:hover": "var:primary" },
    "badge": { "background": "var:primary", "color": "var:white" },
    "search": { "placeholder": { "pl": "Szukaj produktów..." }, "navigate_url": "/search" }
  },
  "settings": {
    "padding_top": 0,
    "padding_bottom": 0,
    "responsive": { "tablet": {}, "mobile": {} }
  }
}
```

## Item Types

| Type | Behavior |
|------|----------|
| `cart` | Renders a badge with the live cart item count. Click toggles a dropdown preview of the cart and links to checkout. |
| `search` | Click toggles a popover containing a search input. As the user types, the Storefront API is queried for autocomplete suggestions. Pressing Enter redirects to `search.navigate_url?q=...`. |
| `account` | Navigates to `item.url` if set, otherwise to the project's configured `commerce.routes.account` (fallback: `/konto`). |
| `custom` | Navigates to `item.url`. If `target_blank` is true, opens in a new tab. |

## Usage Example

```javascript
function renderEcommerceIcons(widget) {
  const { items, config, icon, badge, search } = widget.widget;
  const style = `--size:${config.size}px;--gap:${config.gap}px;--color:${icon.color};`;

  const itemsHtml = items.map((it) => {
    const iconTag = `<i class="${it.icon}" style="font-size:var(--size)"></i>`;
    if (it.type === 'cart') {
      return `<button class="ei-item ei-cart">${iconTag}<span class="ei-badge" data-cart-count>0</span></button>`;
    }
    if (it.type === 'search') {
      return `<button class="ei-item ei-search">${iconTag}</button>`;
    }
    const href = it.url || (it.type === 'account' ? '/konto' : '#');
    const target = it.target_blank ? ' target="_blank" rel="noopener"' : '';
    return `<a class="ei-item" href="${href}"${target}>${iconTag}</a>`;
  }).join('');

  return `<div class="ei" style="${style}">${itemsHtml}</div>`;
}
```
