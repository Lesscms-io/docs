# Customer Account Widget

> **LessCommerce Widget** — This widget requires an active [LessCommerce](https://lesscommerce.io) account connected to your LessCMS project.

A tabbed customer dashboard showing orders, addresses, and profile. Typically placed on the auto-generated system `/account` page.

## Widget Type

```
customer-account
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"customer-account"` |
| `uuid` | string | Unique widget identifier |
| `widget.config` | object | Config element group |
| `widget.config.show_orders` | boolean | Show Orders tab |
| `widget.config.show_addresses` | boolean | Show Addresses tab |
| `widget.config.show_profile` | boolean | Show Profile tab |
| `widget.config.show_logout` | boolean | Show Logout button |
| `widget.heading` | object | Heading element group |
| `widget.heading.text` | string \| object | Page heading (multilingual) |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "customer-account",
  "uuid": "account-001",
  "widget": {
    "config": {
      "show_orders": true,
      "show_addresses": true,
      "show_profile": true,
      "show_logout": true
    },
    "heading": { "text": { "pl": "Moje konto", "en": "My account" } }
  },
  "settings": {
    "padding_top": 30,
    "padding_bottom": 30,
    "responsive": { "tablet": {}, "mobile": {} }
  }
}
```

## Data Source

The widget reads customer data from the LessCommerce API (requires authenticated customer session):

- `GET /api/customers/me` — current customer profile
- `GET /api/customers/me/orders` — list of customer's orders
- `GET /api/customers/me/addresses` — shipping/billing addresses
- `POST /api/customers/logout` — sign out the customer

## Usage Example

```javascript
async function renderCustomerAccount(widget) {
  const { config, heading } = widget.widget;
  const customer = await fetch('/api/customers/me').then(r => r.json());

  if (!customer) {
    window.location.href = '/login';
    return '';
  }

  const tabs = [];
  if (config.show_orders) tabs.push('orders');
  if (config.show_addresses) tabs.push('addresses');
  if (config.show_profile) tabs.push('profile');

  return `
    <section>
      <h1>${heading.text.pl}</h1>
      <nav>${tabs.map(t => `<button data-tab="${t}">${t}</button>`).join('')}</nav>
    </section>
  `;
}
```
