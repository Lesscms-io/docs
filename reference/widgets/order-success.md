# Order Success Widget

> **LessCommerce Widget** — This widget requires an active [LessCommerce](https://lesscommerce.io) account connected to your LessCMS project.

Post-payment confirmation page. Reads `?order=<order_number>` from the URL, fetches the order via the storefront API and renders a confirmation summary. If the gateway reports a failed/cancelled payment, the widget forwards the user to the failure route instead of rendering success UI. Typically placed on the auto-generated system `/zamowienie/sukces` page.

## Widget Type

```
order-success
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"order-success"` |
| `uuid` | string | Unique widget identifier |
| `widget.config` | object | Config element group |
| `widget.config.show_order_number` | boolean | Show the order number in the summary |
| `widget.config.show_items` | boolean | Render the list of ordered items |
| `widget.config.show_totals` | boolean | Render subtotal / shipping / total rows |
| `widget.config.show_shipping_address` | boolean | Render the shipping address block |
| `widget.config.show_payment_status` | boolean | Render the payment status label |
| `widget.config.pending_poll_seconds` | number | Timeout for polling the order status while `payment_status` is `pending`. Default `30`. |
| `widget.config.failure_route` | string | Where to redirect when `payment_status` is failed/cancelled/expired. Default `/zamowienie/niepowodzenie`. |
| `widget.heading` | object | Heading element group |
| `widget.heading.text` | string \| object | Page heading (multilingual) |
| `widget.thank_you_message` | object | Thank-you message element group |
| `widget.thank_you_message.text` | string \| object | Body copy shown above the summary (multilingual) |
| `widget.continue_shopping_button` | object | Continue-shopping CTA element group |
| `widget.continue_shopping_button.text` | string \| object | Button label (multilingual) |
| `widget.continue_shopping_button.href` | string | Button href. Default `/`. |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "order-success",
  "uuid": "order-success-001",
  "widget": {
    "config": {
      "show_order_number": true,
      "show_items": true,
      "show_totals": true,
      "show_shipping_address": true,
      "show_payment_status": true,
      "pending_poll_seconds": 30,
      "failure_route": "/zamowienie/niepowodzenie"
    },
    "heading": { "text": { "pl": "Dziękujemy za zamówienie!", "en": "Thank you for your order!" } },
    "thank_you_message": { "text": { "pl": "Potwierdzenie wyślemy na Twój adres e-mail.", "en": "A confirmation email is on the way." } },
    "continue_shopping_button": {
      "text": { "pl": "Kontynuuj zakupy", "en": "Continue shopping" },
      "href": "/"
    }
  },
  "settings": {
    "padding_top": 30,
    "padding_bottom": 30,
    "responsive": { "tablet": {}, "mobile": {} }
  }
}
```

## Data Source

- `GET /api/orders/by-number/:order_number` — fetches the order. Reads `?order=` from `window.location`.
- While `payment_status === 'pending'`, the widget polls the same endpoint every 3 seconds up to `config.pending_poll_seconds`.
- If `payment_status` returns `failed`, `cancelled`, or `expired`, the widget replaces the URL with `config.failure_route?order=<order_number>`.

## Usage Example

```javascript
async function renderSuccess(widget) {
  const { config } = widget.widget;
  const orderNumber = new URL(window.location.href).searchParams.get('order');
  const { data: order } = await fetch(`/api/orders/by-number/${orderNumber}`).then(r => r.json());

  if (['failed', 'cancelled', 'expired'].includes(order.payment_status)) {
    window.location.replace(`${config.failure_route}?order=${orderNumber}`);
    return;
  }

  // render success UI…
}
```
