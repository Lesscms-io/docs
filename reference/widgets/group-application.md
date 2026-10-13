# Group Application Widget

> **LessCommerce Widget** — This widget requires an active [LessCommerce](https://lesscommerce.io) account connected to your LessCMS project.

A form where a signed-in customer applies to join a customer group with its own prices (for example a "Photographer zone"). Shop staff approve or reject applications in the LessCommerce panel; once approved, the group's price rules apply in the cart.

The group is picked by `config.group_slug`, or the first group that has "Accepts applications" enabled. Guests see a sign-in prompt; members, pending and rejected applicants see their status instead of the form.

## Widget Type

```
group-application
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"group-application"` |
| `uuid` | string | Unique widget identifier |
| `widget.config.group_slug` | string | Slug of the customer group; empty = first group accepting applications |
| `widget.config.show_company` | boolean | Show the company / studio field (required when shown) |
| `widget.config.show_tax_id` | boolean | Show the tax ID field |
| `widget.config.require_tax_id` | boolean | Make the tax ID field required |
| `widget.config.show_phone` | boolean | Show the phone field |
| `widget.config.show_website` | boolean | Show the website / portfolio field |
| `widget.config.show_message` | boolean | Show the free-text message field |
| `widget.heading.text` | object | Multilingual heading |
| `widget.intro.text` | object | Multilingual intro; empty = the group's application intro from the shop |
| `widget.submit_button.text` | object | Multilingual button label |
| `widget.success_message.text` | object | Multilingual message shown after sending |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "group-application",
  "uuid": "ga-101",
  "widget": {
    "config": {
      "group_slug": "fotografowie",
      "show_company": true,
      "show_tax_id": true,
      "require_tax_id": false,
      "show_phone": true,
      "show_website": true,
      "show_message": true
    },
    "heading": { "text": { "pl": "Strefa Fotografa" } },
    "intro": { "text": { "pl": "Fotografowie kupują u nas taniej. Wyślij wniosek." } },
    "submit_button": { "text": { "pl": "Wyślij wniosek" } },
    "success_message": { "text": "" }
  },
  "settings": {
    "padding_top": 20,
    "padding_bottom": 20,
    "responsive": { "tablet": {}, "mobile": {} }
  }
}
```

## Storefront API

The form talks to the LessCommerce Storefront API:

| Method | Path | Auth | Purpose |
|--------|------|------|---------|
| `GET` | `/customers/groups/open` | API key | Groups accepting applications |
| `GET` | `/customers/me/group-applications` | Customer token | Current group + sent applications |
| `POST` | `/customers/me/group-applications` | Customer token | `{ customer_group_uuid, data: { company, tax_id, phone, website, message } }` |

## Usage Example

```javascript
async function applyToGroup(storefront, token, groupUuid, fields) {
  const res = await fetch(`${storefront}/customers/me/group-applications`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json', Authorization: `Bearer ${token}` },
    body: JSON.stringify({ customer_group_uuid: groupUuid, data: fields }),
  });
  if (!res.ok) throw new Error('Application failed');
  return res.json();
}
```
