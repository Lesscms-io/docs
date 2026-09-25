# Product Cart Widget

Displays an "Add to Cart" button on product detail pages with optional quantity input. Used to enable product purchasing from product pages.

## Widget Type

```
product-cart
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"product-cart"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget properties container |
| `widget.button` | object | Button styling element group |
| `widget.button.text` | string | Button label text (multilingual: `{ "en": "...", "pl": "..." }` or plain string) |
| `widget.button.style` | string | Button style: `"primary"`, `"secondary"`, `"outline"` (default: `"primary"`) |
| `widget.button.size` | string | Button size: `"sm"`, `"md"`, `"lg"` (default: `"lg"`) |
| `widget.button.border_radius` | string | Border radius (CSS value, e.g., `"8px"`, `"24px"`, or empty for default) |
| `widget.button.padding` | string\|null | Custom padding override (CSS value, e.g., `"12px 24px"`, or null for default) |
| `widget.button.icon` | string\|null | Icon class name (e.g., `"fa-solid fa-shopping-cart"`, or null for no icon) |
| `widget.button.icon_position` | string | Icon placement: `"left"` or `"right"` (default: `"left"`) |
| `widget.config` | object | Configuration element group |
| `widget.config.show_quantity` | boolean | Display quantity input before cart button (default: `true`) |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "product-cart",
  "uuid": "product-cart-101",
  "widget": {
    "button": {
      "text": {
        "en": "Add to Cart",
        "pl": "Dodaj do koszyka"
      },
      "style": "primary",
      "size": "lg",
      "border_radius": "",
      "padding": null,
      "icon": "fa-solid fa-shopping-cart",
      "icon_position": "left"
    },
    "config": {
      "show_quantity": true
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

## Button Styling

| Property | Default | Notes |
|----------|---------|-------|
| `style` | `"primary"` | Determines button color scheme |
| `size` | `"lg"` | Larger size recommended for e-commerce |
| `border_radius` | `""` | Empty uses CSS default; set `"0"` for square, `"24px"` for rounded |
| `padding` | `null` | Null uses default padding; override with CSS string like `"12px 32px"` |
| `icon` | `null` | FontAwesome icon class or null (no icon) |
| `icon_position` | `"left"` | Icon appears before or after text |

## Quantity Input

When `show_quantity` is true:
- Displays input spinner allowing visitors to select quantity before adding to cart
- Default quantity is 1
- Minimum typically 1, maximum depends on stock/configuration
- Input appears inline or above button depending on layout

When `show_quantity` is false:
- Only the button appears
- Adds quantity of 1 to cart on click

## Button Behavior

On click:
1. Validates product is available
2. Adds selected quantity to cart
3. Shows cart feedback (toast message, mini-cart update, etc.)
4. Optionally redirects to cart page or shows cart drawer

## Text Handling

Button text supports multilingual content. If provided as an object with language keys, the renderer displays text for the current page language. Falls back to English if current language not present.
