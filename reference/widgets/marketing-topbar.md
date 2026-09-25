# Marketing Topbar Widget

Displays a promotional bar at the top of the page linked to a marketing campaign. Renders campaign title, message, and optional CTA. Colors and content are pulled from the campaign configuration; widget settings provide overrides and behavior options.

## Widget Type

```
marketing-topbar
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"marketing-topbar"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget properties container |
| `widget.config` | object | Configuration element group |
| `widget.config.campaign_id` | string\|null | ID of the linked campaign (resolved to campaign data at render time) |
| `widget.config.dismissible` | boolean | Allow visitor to close the topbar (default: `false`) |
| `widget.config.background_color_override` | string\|null | Override campaign background color (hex, var:*, or null for default) |
| `widget.config.text_color_override` | string\|null | Override campaign text color (hex, var:*, or null for default) |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "marketing-topbar",
  "uuid": "marketing-topbar-456",
  "widget": {
    "config": {
      "campaign_id": "summer-sale-2024",
      "dismissible": true,
      "background_color_override": null,
      "text_color_override": null
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

## Campaign Data Source

Campaign content (title, message, CTA text, link, colors) is loaded from the campaign system at render time using the `campaign_id`. If the campaign is not found or is inactive, the widget renders as empty.

## Dismissible Behavior

When `dismissible` is true, the topbar includes a close button. Dismissed state is stored in browser localStorage and persists across page navigation.

## Color Overrides

Color overrides allow fine-tuning the topbar appearance without modifying the underlying campaign. Uses standard LessCMS color format: hex colors (`#ffffff`), CSS variables (`var:primary`), or null (use campaign default).
