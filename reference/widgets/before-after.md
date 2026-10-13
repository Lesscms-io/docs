# Before / After Widget (premium)

Compares images under a draggable divider. Two modes:

- `standard` (default): two photos (`before` / `after`), divider (horizontal or vertical).
- `thermal`: one photo (`after.image`), cold layer on the left and hot layer on the right
  of the divider (thermal-vision look). Optional colour scale below the frame.

Requires the `premium_widgets` plan feature.

## Widget Type

```
before-after
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"before-after"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget properties |
| `widget.before` | object | "Before" image (standard mode only; ignored in thermal mode) |
| `widget.before.image` | string | Image URL |
| `widget.before.alt` | object | Multilingual alt text |
| `widget.after` | object | "After" image (standard) / the single photo (thermal) |
| `widget.after.image` | string | Image URL |
| `widget.after.alt` | object | Multilingual alt text |
| `widget.config` | object | Behaviour |
| `widget.config.mode` | string | `"standard"` or `"thermal"` |
| `widget.config.orientation` | string | `"horizontal"` (divider moves left/right) or `"vertical"` |
| `widget.config.start_position` | number | Initial divider position, `0`–`100` % (default `50`) |
| `widget.config.hover_move` | boolean | Divider follows the pointer without dragging |
| `widget.config.auto_demo` | boolean | Divider sweeps once automatically when the widget scrolls into view (skipped with `prefers-reduced-motion`) |
| `widget.config.ratio` | string | Aspect ratio: `"16:9"`, `"4:3"`, `"3:2"`, `"1:1"`, `"21:9"` or `"auto"` |
| `widget.config.radius` | number | Corner radius in px (default `12`) |
| `widget.config.labels` | boolean | Show the badges |
| `widget.config.show_scale` | boolean | Thermal mode: show the colour scale under the frame (default `true`) |
| `widget.labels` | object | Badges |
| `widget.labels.before` | object | Multilingual left / cold badge text (falls back to `before.label`) |
| `widget.labels.after` | object | Multilingual right / hot badge text (falls back to `after.label`) |
| `widget.labels.background` | string | Badge background |
| `widget.labels.color` | string | Badge text color |
| `widget.labels.position` | string | `"top"`, `"center"`, `"bottom"` (default `"bottom"`, thermal `"top"`) |
| `widget.handle` | object | Divider and its grip |
| `widget.handle.style` | string | `"circle"`, `"bar"`, `"minimal"` (line only) |
| `widget.handle.background` | string\|null | Grip fill (`null` = use `handle.color`, legacy) |
| `widget.handle.color` | string\|null | Grip arrow color (applies when `handle.background` is set) |
| `widget.handle.color:hover` | string\|null | Grip fill on hover |
| `widget.handle.width` | number | Divider line width in px (default `3`) |
| `widget.handle.size` | number | Grip size in px (default `44`) |
| `widget.thermal` | object | Thermal mode options |
| `widget.thermal.intensity` | number | Strength of the thermal colours, `0`–`100` % (default `100`) |
| `widget.thermal.min_label` | object | Multilingual scale label, cold end (default "zimno" / "cold") |
| `widget.thermal.max_label` | object | Multilingual scale label, hot end (default "ciepło" / "hot") |
| `settings` | object | Style settings (padding, background, border, hover transforms) |

The thermal palette is fixed (cold: blue/violet, hot: yellow → orange → red → violet);
only `thermal.intensity` changes it.

## Backward compatibility

Widgets saved before the `mode` / `labels` / `thermal` options keep rendering unchanged:
`before.label` / `after.label` texts, `config.start`, `label` group and `handle.color` as the
grip fill are still read and returned.

## Example Response

```json
{
  "widget_type": "before-after",
  "uuid": "before-after-123",
  "widget": {
    "before": { "image": "https://img.lesscms.io/…/kitchen-old.jpg", "alt": { "en": "Kitchen before renovation", "pl": "Kuchnia przed remontem" } },
    "after": { "image": "https://img.lesscms.io/…/kitchen-new.jpg", "alt": { "en": "Kitchen after renovation", "pl": "Kuchnia po remoncie" } },
    "config": { "mode": "standard", "orientation": "horizontal", "start_position": 50, "hover_move": false, "auto_demo": true, "ratio": "16:9", "radius": 12, "labels": true, "show_scale": true },
    "labels": { "before": { "en": "Before", "pl": "Przed" }, "after": { "en": "After", "pl": "Po" }, "background": "var:dark:70", "color": "#ffffff", "position": "bottom" },
    "handle": { "style": "circle", "background": "#ffffff", "color": "#111827", "color:hover": null, "width": 3, "size": 44 },
    "thermal": { "intensity": 100, "min_label": {}, "max_label": {} }
  },
  "settings": {}
}
```

Thermal example (`config.mode: "thermal"`): only `after.image` is used; `labels.before` /
`labels.after` read as "Bez izolacji" / "Po natrysku PUR"; `thermal.min_label` / `max_label`
label the scale.

## Rendering Notes

- Stack both images absolutely in a frame with the aspect ratio; clip the *before* layer with `clip-path: inset(...)` driven by the divider position (standard). In thermal mode, clip the hot layer from the divider to the right edge.
- Use pointer events (`touch-action: none`) for drag; expose the divider as `role="slider"` with arrow-key support.
- `auto_demo`: animate the divider once (about 2.2 s, ±32 % around the start position) when the widget is at least 50 % visible; stop it on the first user interaction.
- `ratio: "auto"` lets the *after* image define the height.
