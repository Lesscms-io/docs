# Business Info Widget

Displays business contact information (name, phone, email, address, hours, tax ID, social links) pulled from project settings. Each field visibility is controlled independently via widget configuration.

## Widget Type

```
business-info
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"business-info"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget properties container |
| `widget.show_name` | boolean | Display business name (default: `true`) |
| `widget.show_phone` | boolean | Display phone number (default: `true`) |
| `widget.show_email` | boolean | Display email address (default: `true`) |
| `widget.show_address` | boolean | Display full address (default: `true`) |
| `widget.show_hours` | boolean | Display business hours (default: `true`) |
| `widget.show_tax_id` | boolean | Display tax/VAT ID (default: `false`) |
| `widget.show_social` | boolean | Display social media links (default: `false`) |
| `widget.layout` | string | Display layout: `"stacked"` (vertical list) or inline (default: `"stacked"`) |
| `widget.label_phone` | string\|null | Custom label for phone field (multilingual: `{ "en": "...", "pl": "..." }` or plain string) |
| `widget.label_email` | string\|null | Custom label for email field (multilingual or plain string) |
| `widget.label_address` | string\|null | Custom label for address field (multilingual or plain string) |
| `widget.label_hours` | string\|null | Custom label for hours field (multilingual or plain string) |
| `widget.label_tax_id` | string\|null | Custom label for tax ID field (multilingual or plain string) |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "business-info",
  "uuid": "business-info-123",
  "widget": {
    "show_name": true,
    "show_phone": true,
    "show_email": true,
    "show_address": true,
    "show_hours": true,
    "show_tax_id": false,
    "show_social": false,
    "layout": "stacked",
    "label_phone": null,
    "label_email": null,
    "label_address": null,
    "label_hours": null,
    "label_tax_id": null
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

## Data Source

All actual business data (name, phone, email, address, hours, tax ID, social links) is pulled from the project's business settings at render time. The widget configuration only controls which fields are displayed. This ensures business information is centralized and updates automatically across all instances.

## Layout Behavior

- **Stacked**: Each information field displays on its own line
- **Inline**: Fields display horizontally (space-permitting)

## Custom Labels

Optional custom labels can be provided for each field. Labels support multilingual content (same format as page content) and are displayed before the actual value.
