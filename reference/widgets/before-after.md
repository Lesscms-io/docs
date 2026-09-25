# Before / After Widget (premium)

Two images compared under a draggable divider (horizontal or vertical) —
renovations, retouching, product upgrades. Requires the `premium_widgets`
plan feature.

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
| `widget.before` | object | "Before" image (left / top) |
| `widget.before.image` | string | Image URL |
| `widget.before.label` | object | Multilingual badge label (e.g. "Before") |
| `widget.before.alt` | object | Multilingual alt text |
| `widget.after` | object | "After" image (right / bottom) |
| `widget.after.image` | string | Image URL |
| `widget.after.label` | object | Multilingual badge label |
| `widget.after.alt` | object | Multilingual alt text |
| `widget.config` | object | Behaviour |
| `widget.config.orientation` | string | `"horizontal"` (divider moves left/right) or `"vertical"` |
| `widget.config.start` | number | Initial divider position, `0`–`100` % (default `50`) |
| `widget.config.hover_move` | boolean | Divider follows the pointer without dragging |
| `widget.config.ratio` | string | Aspect ratio: `"16:9"`, `"4:3"`, `"3:2"`, `"1:1"`, `"21:9"` or `"auto"` |
| `widget.config.radius` | number | Corner radius in px (default `12`) |
| `widget.config.labels` | boolean | Show the before / after badges |
| `widget.handle` | object | Divider and its grip |
| `widget.handle.style` | string | `"circle"`, `"bar"`, `"minimal"` (line only) |
| `widget.handle.color` | string | Divider / grip color |
| `widget.handle.color:hover` | string\|null | Divider / grip color on hover |
| `widget.handle.width` | number | Divider line width in px (default `3`) |
| `widget.handle.size` | number | Grip size in px (default `44`) |
| `widget.label` | object | Badge look |
| `widget.label.background` | string | Badge background |
| `widget.label.color` | string | Badge text color |
| `widget.label.position` | string | `"top"`, `"center"`, `"bottom"` |
| `settings` | object | Style settings (padding, background, border, hover transforms) |

## Example Response

```json
{
  "widget_type": "before-after",
  "uuid": "before-after-123",
  "widget": {
    "before": { "image": "https://img.lesscms.io/…/kitchen-old.jpg", "label": { "en": "Before", "pl": "Przed" }, "alt": { "en": "Kitchen before renovation", "pl": "Kuchnia przed remontem" } },
    "after": { "image": "https://img.lesscms.io/…/kitchen-new.jpg", "label": { "en": "After", "pl": "Po" }, "alt": { "en": "Kitchen after renovation", "pl": "Kuchnia po remoncie" } },
    "config": { "orientation": "horizontal", "start": 50, "hover_move": false, "ratio": "16:9", "radius": 12, "labels": true },
    "handle": { "style": "circle", "color": "#ffffff", "color:hover": null, "width": 3, "size": 44 },
    "label": { "background": "var:dark:70", "color": "#ffffff", "position": "bottom" }
  },
  "settings": {}
}
```

## Rendering Notes

- Stack both images absolutely in a frame with the aspect ratio; clip the *before* layer with `clip-path: inset(...)` driven by the divider position.
- Use pointer events (`touch-action: none`) for drag; expose the divider as `role="slider"` with arrow-key support.
- `ratio: "auto"` lets the *after* image define the height.
