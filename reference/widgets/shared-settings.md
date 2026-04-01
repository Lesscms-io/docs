# Shared Widget Settings

> **Note:** This is a meta documentation page describing the shared `settings` object common to all widgets. It is not a widget definition. Every widget's API response includes these settings automatically.

Every widget returns a `settings` object with common style properties extracted from `widget.data.style`. These are managed by the WidgetSettingsPanel (shared across all widgets).

## Settings Properties

| Property | Type | Description |
|----------|------|-------------|
| `settings.padding_top` | number | Top padding (px) |
| `settings.padding_right` | number | Right padding (px) |
| `settings.padding_bottom` | number | Bottom padding (px) |
| `settings.padding_left` | number | Left padding (px) |
| `settings.margin_top` | number | Top margin (px) |
| `settings.margin_right` | number | Right margin (px) |
| `settings.margin_bottom` | number | Bottom margin (px) |
| `settings.margin_left` | number | Left margin (px) |
| `settings.background_color` | string\|null | Background color |
| `settings.background_opacity` | number | Background opacity (0-100) |
| `settings.background_image` | string\|null | Background image URL |
| `settings.background_size` | string | Background size (cover, contain, etc.) |
| `settings.background_position` | string | Background position |
| `settings.border_radius` | number | Border radius (px) |
| `settings.border_width` | number | Border width (px) |
| `settings.border_color` | string\|null | Border color |
| `settings.border_style` | string | Border style (solid, dashed, etc.) |
| `settings.box_shadow` | string\|null | Box shadow CSS |
| `settings.width` | number\|null | Fixed width (px) |
| `settings.max_width` | number\|null | Maximum width (px) |
| `settings.height` | number\|null | Fixed height (px) |
| `settings.min_height` | number\|null | Minimum height (px) |
| `settings.vertical_align` | string | Vertical alignment (flex-start, center, flex-end) |
| `settings.horizontal_align` | string | Horizontal alignment (stretch, flex-start, center, flex-end) |
| `settings.hidden` | boolean | Hidden on desktop |
| `settings.css_id` | string\|null | Custom CSS ID |
| `settings.css_class` | string\|null | Custom CSS class |
| `settings.link` | object\|null | Widget clickability link |
| `settings.animation_type` | string\|null | Scroll animation type |
| `settings.animation_duration` | number | Animation duration (ms) |
| `settings.animation_delay` | number | Animation delay (ms) |
| `settings.responsive` | object | Responsive overrides per breakpoint |
| `settings.responsive.tablet` | object | Tablet overrides |
| `settings.responsive.mobile` | object | Mobile overrides |
| `settings.responsive.tablet.hidden` | boolean | Hidden on tablet |
| `settings.responsive.mobile.hidden` | boolean | Hidden on mobile |

These settings are always present in the API response and do not need to be documented per widget.

## Example Response

```json
{
  "settings": {
    "paddingTop": 20,
    "paddingRight": 16,
    "paddingBottom": 20,
    "paddingLeft": 16,
    "marginTop": 0,
    "marginBottom": 0,
    "backgroundColor": "var:white",
    "backgroundOpacity": 100,
    "borderRadius": 8,
    "borderWidth": 1,
    "borderColor": "var:border",
    "borderStyle": "solid",
    "boxShadow": null,
    "verticalAlign": "flex-start",
    "horizontalAlign": "stretch",
    "hidden": false,
    "responsive": {
      "tablet": {
        "paddingTop": 16,
        "paddingRight": 12,
        "paddingBottom": 16,
        "paddingLeft": 12,
        "hidden": false
      },
      "mobile": {
        "paddingTop": 12,
        "paddingRight": 8,
        "paddingBottom": 12,
        "paddingLeft": 8,
        "hidden": false
      }
    }
  }
}
```
