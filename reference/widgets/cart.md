# Cart Widget

> **LessCommerce Widget** — This widget requires an active [LessCommerce](https://lesscommerce.io) account connected to your LessCMS project.

A full-page shopping cart widget displaying all items, totals, coupons and the checkout call-to-action. Typically placed on the auto-generated system `/cart` page but can be used on any custom route.

## Widget Type

```
cart
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"cart"` |
| `uuid` | string | Unique widget identifier |
| `widget.config` | object | Config element group |
| `widget.config.show_coupons` | boolean | Show coupon input field |
| `widget.config.show_shipping_estimate` | boolean | Show estimated shipping cost |
| `widget.config.empty_redirect` | string | Route to redirect to when cart is empty |
| `widget.heading` | object | Heading element group |
| `widget.heading.text` | string \| object | Page heading (multilingual) |
| `widget.empty_message` | object | Empty message element group |
| `widget.empty_message.text` | string \| object | Empty cart message (multilingual) |
| `widget.checkout_button` | object | Checkout button element group |
| `widget.checkout_button.text` | string \| object | Checkout button label (multilingual) |
| `widget.continue_shopping` | object | Continue shopping link element group |
| `widget.continue_shopping.text` | string \| object | Continue shopping link label (multilingual) |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "cart",
  "uuid": "cart-001",
  "widget": {
    "config": {
      "show_coupons": true,
      "show_shipping_estimate": true,
      "empty_redirect": "/"
    },
    "heading": { "text": { "pl": "Koszyk", "en": "Cart" } },
    "empty_message": { "text": { "pl": "Koszyk jest pusty", "en": "Your cart is empty" } },
    "checkout_button": { "text": { "pl": "Do zamówienia", "en": "Checkout" } },
    "continue_shopping": { "text": { "pl": "Kontynuuj zakupy", "en": "Continue shopping" } }
  },
  "settings": {
    "padding_top": 30,
    "padding_bottom": 30,
    "responsive": { "tablet": {}, "mobile": {} }
  }
}
```

## Data Source

The widget reads the live cart state from the LessCommerce API at render time:

- `GET /api/cart` — returns the active cart for the current customer (or guest session)
- `PUT /api/cart/items/{itemId}` — update item quantity
- `DELETE /api/cart/items/{itemId}` — remove item
- `POST /api/cart/coupons` — apply coupon code (when `show_coupons` is `true`)

## Usage Example

```javascript
async function renderCart(widget) {
  const { config, heading, empty_message, checkout_button, continue_shopping } = widget.widget;
  const cart = await fetch('/api/cart').then(r => r.json());

  if (!cart.items?.length) {
    return `
      <section>
        <h1>${heading.text.pl}</h1>
        <p>${empty_message.text.pl}</p>
        <a href="${config.empty_redirect}">${continue_shopping.text.pl}</a>
      </section>
    `;
  }

  return `
    <section>
      <h1>${heading.text.pl}</h1>
      <ul>${cart.items.map(renderItem).join('')}</ul>
      <button>${checkout_button.text.pl}</button>
    </section>
  `;
}
```
