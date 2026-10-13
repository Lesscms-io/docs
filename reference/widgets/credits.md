# Credits Widget

A footer credit line: "Made by [Softmasters logo] · Running on [LessCMS logo]". Logos are inline SVG outlines (no font dependency), colored via `currentColor`.

## Widget Type

```
credits
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"credits"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget data |
| `widget.text` | object | Text element-group |
| `widget.text.made_by` | object | Multilingual label before the creator logo (default `"Made by"`) |
| `widget.text.running_on` | object | Multilingual label before the platform logo (default `"Running on"`) |
| `widget.text.color` | string\|null | Text color (`null` = inherit, rendered muted) |
| `widget.text.size` | number | Text size in px (default: `13`) |
| `widget.logo` | object | Logo element-group |
| `widget.logo.color` | string\|null | Logo color (`null` = inherit text color) |
| `widget.logo.color:hover` | string\|null | Logo color on hover (default: `"var:primary"`) |
| `widget.logo.height` | number | Logo height in px (default: `16`) |
| `widget.logo.accent` | boolean | Green Softmasters stroke `#C6E638` (default: `true`) |
| `widget.config` | object | Config element-group |
| `widget.config.show_made_by` | boolean | Show the creator item (default: `true`) |
| `widget.config.show_running_on` | boolean | Show the platform item (default: `true`) |
| `widget.config.alignment` | string | `"left"`, `"center"` or `"right"` (default: `"center"`) |
| `widget.config.separator` | string | `""`, `"·"` or `"|"` (default: `""`, 28px gap) |
| `widget.config.layout` | string | `"inline"` (one line) or `"stacked"` (one item per line) |
| `widget.config.link_made_by` | string\|null | URL of the creator logo (default: `"https://softmasters.pl/"`) |
| `widget.config.link_running_on` | string\|null | URL of the platform logo (default: `"https://lesscms.io/"`) |
| `widget.config.target_blank` | boolean | Open links in a new tab (default: `true`) |
| `settings` | object | [Shared style settings](shared-settings.md) |

On narrow screens (max-width 480px) the items are stacked one below another, centered.

## Example Response

```json
{
  "widget_type": "credits",
  "uuid": "credits-123",
  "widget": {
    "text": {
      "made_by": { "pl": "Made by", "en": "Made by" },
      "running_on": { "pl": "Running on", "en": "Running on" },
      "color": null,
      "size": 13
    },
    "logo": {
      "color": null,
      "color:hover": "var:primary",
      "height": 16,
      "accent": true
    },
    "config": {
      "show_made_by": true,
      "show_running_on": true,
      "alignment": "center",
      "separator": "",
      "layout": "inline",
      "link_made_by": "https://softmasters.pl/",
      "link_running_on": "https://lesscms.io/",
      "target_blank": true
    }
  },
  "settings": {
    "padding_top": 16,
    "padding_bottom": 16
  }
}
```

## Pitfalls

- Labels are multilingual objects. If the current language has no value, the renderer falls back to the default English label.
- `logo.color` set to `null` inherits the text color, so the logos follow the footer theme without extra configuration.
