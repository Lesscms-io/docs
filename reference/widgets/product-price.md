# Product Price Widget

Displays product price on product detail pages with support for sale/compare pricing and discount badge. Renders the current price prominently, with optional strikethrough original price and percentage-off badge.

## Widget Type

```
product-price
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"product-price"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget properties container |
| `widget.config` | object | Configuration element group |
| `widget.config.field_price` | string | Product field containing current price (default: `"price"`) |
| `widget.config.field_compare_price` | string | Product field containing original/list price for comparison (default: `"compare_at_price"`) |
| `widget.price` | object | Price styling element group |
| `widget.price.color` | string\|null | Current price text color (default: `"var:primary"`) |
| `widget.price.font_size` | number | Current price font size in pixels (default: `32`) |
| `widget.price.font_weight` | string | Current price font weight: `"600"`, `"700"`, `"800"` (default: `"800"`) |
| `widget.compare` | object | Compare price styling element group |
| `widget.compare.color` | string\|null | Compare/original price text color (default: `"var:muted"`) |
| `widget.compare.font_size` | number | Compare price font size in pixels (default: `16`) |
| `widget.badge` | object | Badge styling element group |
| `widget.badge.background` | string | Badge background color (default: `"var:danger"`) |
| `widget.badge.color` | string | Badge text color (default: `"var:white"`) |
| `widget.badge.show` | boolean | Display discount percentage badge (default: `true`) |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "product-price",
  "uuid": "product-price-789",
  "widget": {
    "config": {
      "field_price": "price",
      "field_compare_price": "compare_at_price"
    },
    "price": {
      "color": "var:primary",
      "font_size": 32,
      "font_weight": "800"
    },
    "compare": {
      "color": "var:muted",
      "font_size": 16
    },
    "badge": {
      "background": "var:danger",
      "color": "var:white",
      "show": true
    }
  },
  "settings": {
    "padding_top": 8,
    "padding_bottom": 8,
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Price Display Behavior

**Current Price Only:**
- Displays the main price in large, bold text
- Used when no compare price is available

**With Compare Price (Sale):**
- Current price displayed prominently in primary color
- Compare price shown in muted color with strikethrough
- Discount badge showing percentage off (e.g., "20%")
- Badge appears only if `badge.show` is true and compare price > current price

## Price Format

Prices are returned as numbers (in cents/grosze):
- Example: `2999` = $29.99 or 29,99 PLN
- Renderer divides by 100 for display
- Currency symbol determined by shop configuration

## Discount Calculation

Discount percentage badge is calculated as:
```
discount_percent = ((compare_price - price) / compare_price) * 100
```

Badge is hidden if:
- `badge.show` is false
- Compare price is not provided or equals current price
- Compare price is less than current price (invalid state)

## Field Paths

Use product field paths to fetch prices from nested structures:
- `price` – Simple price field
- `variants[0].price` – Price from first variant (if using variant structure)
- `priceInCents` – If your shop stores prices in cents directly
