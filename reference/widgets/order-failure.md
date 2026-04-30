# Order Failure Widget

> **LessCommerce Widget** — This widget requires an active [LessCommerce](https://lesscommerce.io) account connected to your LessCMS project.

Post-payment failure page. Reads `?order=<order_number>` from the URL, fetches the order to display context, and offers a retry / contact CTA. The user lands here when the [order-success](order-success.md) widget detects a failed/cancelled/expired `payment_status`. Typically placed on the auto-generated system `/zamowienie/niepowodzenie` page.

## Widget Type

```
order-failure
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"order-failure"` |
| `uuid` | string | Unique widget identifier |
| `widget.config` | object | Config element group |
| `widget.config.show_order_number` | boolean | Show the order number |
| `widget.config.show_failure_reason` | boolean | Render the failure reason label |
| `widget.config.show_retry_button` | boolean | Render the retry CTA |
| `widget.config.show_contact_info` | boolean | Render the contact-info paragraph |
| `widget.config.retry_route` | string | Destination of the retry button. Default `/zamowienie`. |
| `widget.heading` | object | Heading element group |
| `widget.heading.text` | string \| object | Page heading (multilingual) |
| `widget.failure_message` | object | Failure message element group |
| `widget.failure_message.text` | string \| object | Body copy explaining the situation (multilingual) |
| `widget.retry_button` | object | Retry button element group |
| `widget.retry_button.text` | string \| object | Retry button label (multilingual) |
| `widget.contact_info` | object | Contact info element group |
| `widget.contact_info.text` | string \| object | Contact paragraph (multilingual) |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "order-failure",
  "uuid": "order-failure-001",
  "widget": {
    "config": {
      "show_order_number": true,
      "show_failure_reason": true,
      "show_retry_button": true,
      "show_contact_info": true,
      "retry_route": "/zamowienie"
    },
    "heading": { "text": { "pl": "Płatność nieudana", "en": "Payment failed" } },
    "failure_message": { "text": { "pl": "Spróbuj ponownie lub skontaktuj się z nami.", "en": "Try again or contact us." } },
    "retry_button": { "text": { "pl": "Ponów płatność", "en": "Retry payment" } },
    "contact_info": { "text": { "pl": "kontakt@sklep.pl", "en": "support@shop.com" } }
  },
  "settings": {
    "padding_top": 30,
    "padding_bottom": 30,
    "responsive": { "tablet": {}, "mobile": {} }
  }
}
```

## Data Source

- `GET /api/orders/by-number/:order_number` — fetches the order to display its number and derive the failure reason from `payment_status`. Reads `?order=` from `window.location`.
- The retry button is a plain anchor to `config.retry_route` — no extra API call.

## Usage Example

```javascript
async function renderFailure(widget) {
  const { config } = widget.widget;
  const orderNumber = new URL(window.location.href).searchParams.get('order');
  const { data: order } = await fetch(`/api/orders/by-number/${orderNumber}`).then(r => r.json());

  const reasonKey = {
    failed: 'Płatność odrzucona',
    cancelled: 'Płatność anulowana',
    expired: 'Płatność wygasła',
  }[order.payment_status] || 'Nieznany powód';

  // render failure UI with reasonKey and retry link to config.retry_route…
}
```
