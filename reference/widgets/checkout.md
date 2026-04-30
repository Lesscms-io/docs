# Checkout Widget

> **LessCommerce Widget** — This widget requires an active [LessCommerce](https://lesscommerce.io) account connected to your LessCMS project.

A full checkout flow widget with multi-step form (shipping, payment, summary). Typically placed on the auto-generated system `/checkout` page.

## Widget Type

```
checkout
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"checkout"` |
| `uuid` | string | Unique widget identifier |
| `widget.config` | object | Config element group |
| `widget.config.require_login` | boolean | Require customer login before checkout |
| `widget.config.show_progress_bar` | boolean | Show multi-step progress bar |
| `widget.config.success_route` | string | Route to redirect to after the customer returns from the payment gateway. The [order-success](order-success.md) widget on that page decides whether to render success UI or forward the user to the failure route. Default `/zamowienie/sukces`. |
| `widget.config.failure_route` | string | Fallback route used by the success widget when `payment_status` is failed/cancelled. Default `/zamowienie/niepowodzenie`. |
| `widget.heading` | object | Heading element group |
| `widget.heading.text` | string \| object | Page heading (multilingual) |
| `widget.submit_button` | object | Submit button element group |
| `widget.submit_button.text` | string \| object | Submit button label (multilingual) |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "checkout",
  "uuid": "checkout-001",
  "widget": {
    "config": {
      "require_login": false,
      "show_progress_bar": true,
      "success_route": "/zamowienie/sukces",
      "failure_route": "/zamowienie/niepowodzenie"
    },
    "heading": { "text": { "pl": "Zamówienie", "en": "Checkout" } },
    "submit_button": { "text": { "pl": "Złóż zamówienie", "en": "Place order" } }
  },
  "settings": {
    "padding_top": 30,
    "padding_bottom": 30,
    "responsive": { "tablet": {}, "mobile": {} }
  }
}
```

## Data Source

The widget reads the live cart state and submits the order via the LessCommerce API:

- `GET /api/cart/validate` — validate cart before showing the form (stock, prices, eligibility)
- `POST /api/cart/checkout` — submit the order with shipping + payment details
- On success: redirect to `widget.config.success_route` (passing `?order=<order_number>`). The success widget verifies `payment_status` client-side and forwards to `widget.config.failure_route` if the gateway reported failure.

## Usage Example

```javascript
async function submitCheckout(widget, formData) {
  const { config } = widget.widget;
  const response = await fetch('/api/cart/checkout', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(formData),
  });

  if (response.ok) {
    const data = await response.json();
    window.location.href = `${config.success_route}?order=${data.order_number}`;
  }
}
```
