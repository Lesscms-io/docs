# Mini Cart Widget

> **LessCommerce Widget** — This widget requires an active [LessCommerce](https://lesscommerce.io) account connected to your LessCMS project.

A compact cart indicator that displays the current cart state. Supports multiple display styles and can open a dropdown preview or navigate to the full cart page.

## Widget Type

```
mini-cart
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"mini-cart"` |
| `uuid` | string | Unique widget identifier |
| `widget.config` | object | Config element group |
| `widget.config.style` | string | Display style: `"icon-badge"`, `"icon-text"`, or `"icon-only"` |
| `widget.config.show_total` | boolean | Whether to display the cart total amount |
| `widget.config.click_action` | string | Action on click: `"dropdown"` (show cart preview) or `"navigate"` (go to cart page) |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "mini-cart",
  "uuid": "mc-456",
  "widget": {
    "config": {
      "style": "icon-badge",
      "show_total": true,
      "click_action": "dropdown"
    }
  },
  "settings": {
    "padding_top": 0,
    "padding_bottom": 0,
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Display Styles

| Value | Description |
|-------|-------------|
| `icon-badge` | Cart icon with a badge showing the number of items |
| `icon-text` | Cart icon with text label showing item count |
| `icon-only` | Cart icon only, no count indicator |

## Click Actions

| Value | Description |
|-------|-------------|
| `dropdown` | Opens a mini cart dropdown with item preview |
| `navigate` | Navigates directly to the full cart page |

## Usage Example

```javascript
function renderMiniCart(widget) {
  const { config } = widget.widget;
  const cartIcon = '<i class="fa-solid fa-cart-shopping"></i>';

  if (config.style === 'icon-badge') {
    return `<button class="mini-cart">${cartIcon}<span class="badge" data-cart-count>0</span></button>`;
  } else if (config.style === 'icon-text') {
    return `<button class="mini-cart">${cartIcon} <span data-cart-count>0</span> items</button>`;
  } else {
    return `<button class="mini-cart">${cartIcon}</button>`;
  }
}
```
