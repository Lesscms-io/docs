# Campaign Banner Widget

Displays a promotional banner linked to a marketing campaign with optional image, title, subtitle, and call-to-action button. Banner placement type controls styling and layout (hero, category strip, cart drawer, or thank-you page).

## Widget Type

```
campaign-banner
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"campaign-banner"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget properties container |
| `widget.config` | object | Configuration element group |
| `widget.config.placement` | string | Banner context: `"hero"`, `"category_strip"`, `"cart_drawer"`, `"thank_you"` (default: `"hero"`) |
| `widget.config.campaign_id` | string\|null | ID of the linked campaign (resolved to campaign data at render time) |
| `widget.config.show_title` | boolean | Display campaign title (default: `true`) |
| `widget.config.show_subtitle` | boolean | Display campaign subtitle/message (default: `true`) |
| `widget.config.show_cta` | boolean | Display call-to-action button (default: `true`) |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "campaign-banner",
  "uuid": "campaign-banner-789",
  "widget": {
    "config": {
      "placement": "hero",
      "campaign_id": "summer-sale-2024",
      "show_title": true,
      "show_subtitle": true,
      "show_cta": true
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

## Placement Types

| Placement | Use Case | Styling |
|-----------|----------|---------|
| `hero` | Full-width banner at top of page | Large height, prominent text, full background image |
| `category_strip` | Thin banner in category list | Compact height, small text, icon-only option |
| `cart_drawer` | Banner inside cart/checkout modal | Sidebar-width, inline with products |
| `thank_you` | Post-purchase confirmation page | Medium size, success styling |

## Campaign Data Source

Campaign content (title, subtitle, image, CTA text, link, colors) is loaded from the campaign system at render time using the `campaign_id`. Campaign display settings automatically adapt based on placement type.

## Content Visibility

Each content element (title, subtitle, CTA) can be hidden independently via configuration, allowing fine-grained control over what appears in each placement context without duplicating campaigns.
