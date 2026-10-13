# Marquee Widget

A full-width band with an endless horizontal scroll. Two modes:

- `text` — big words (e.g. "BUDUJEMY / MODERNIZUJEMY / IZOLUJEMY"), every other word can be an outline, separators in the accent color.
- `logos` — a row of partner logos, optionally monochrome, fading in on hover.

The loop is seamless: the renderer outputs two identical groups and animates `translateX(-50%)`. `prefers-reduced-motion` turns the band into a static row.

## Widget Type

```
marquee
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"marquee"` |
| `uuid` | string | Unique widget identifier |
| `widget.config` | object | Scroll behavior |
| `widget.config.mode` | string | `text` or `logos` |
| `widget.config.speed` | number | Seconds for one full loop (default `28`) |
| `widget.config.direction` | string | `left` or `right` |
| `widget.config.pause_on_hover` | boolean | Pause the scroll while hovered (default `true`) |
| `widget.config.gap` | number | Space between items in px (default `60`) |
| `widget.config.fade_edges` | boolean | Fade the band out at both edges (default `true`) |
| `widget.text` | object | Text mode styling |
| `widget.text.color` | string\|null | Text color |
| `widget.text.color:hover` | string\|null | Text color while the band is hovered |
| `widget.text.outline_alternate` | boolean | Every other item drawn as outline (default `true`) |
| `widget.text.outline_color` | string\|null | Outline color (defaults to the text color) |
| `widget.text.separator` | string | Separator between items (default `/`) |
| `widget.text.separator_color` | string\|null | Separator color (default `var:primary`) |
| `widget.text.text_style` | string\|null | Project text style code (`lcms-ts-<code>` class on root) |
| `widget.text.tag` | string | HTML tag for each item (`span`, `p`, `div`, `h2`, `h3`, `h4`) |
| `widget.logo` | object | Logos mode styling |
| `widget.logo.height` | number | Logo height in px (default `54`, about 2/3 on mobile) |
| `widget.logo.mono` | string | `none`, `light` (white) or `dark` (black) |
| `widget.logo.opacity` | number | Opacity in percent (default `60`) |
| `widget.logo.opacity:hover` | number | Opacity on hover in percent (default `100`) |
| `widget.border` | object | Lines above and below the band |
| `widget.border.top` | boolean | Top line (default `true`) |
| `widget.border.bottom` | boolean | Bottom line (default `true`) |
| `widget.border.color` | string\|null | Line color |
| `widget.items` | array | Elements of the band |
| `widget.items[].text` | object | Multilingual text (text mode) |
| `widget.items[].image` | string\|null | Logo URL (logos mode; items without an image are skipped) |
| `widget.items[].alt` | object | Multilingual alt text of the logo |
| `widget.items[].link` | string\|null | Optional URL wrapping the logo |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "marquee",
  "uuid": "mq-001",
  "widget": {
    "config": {
      "mode": "text",
      "speed": 28,
      "direction": "left",
      "pause_on_hover": true,
      "gap": 60,
      "fade_edges": true
    },
    "text": {
      "color": null,
      "color:hover": null,
      "outline_alternate": true,
      "outline_color": null,
      "separator": "/",
      "separator_color": "var:primary",
      "text_style": null,
      "tag": "span"
    },
    "logo": {
      "height": 54,
      "mono": "none",
      "opacity": 60,
      "opacity:hover": 100
    },
    "border": {
      "top": true,
      "bottom": true,
      "color": null
    },
    "items": [
      { "text": { "pl": "Budujemy" }, "image": null, "alt": {}, "link": null },
      { "text": { "pl": "Modernizujemy" }, "image": null, "alt": {}, "link": null },
      { "text": { "pl": "Izolujemy" }, "image": null, "alt": {}, "link": null }
    ]
  },
  "settings": {}
}
```

## Rendering Notes

- Each copy of the item list is repeated until it has at least 12 slots (an even number), so one copy always covers a wide screen and the outline alternation continues across the loop seam.
- Repeated slots carry the `is-dup` class and are hidden under `prefers-reduced-motion`, where only the original items form a centered row.
- Logos mode renders only items that have an `image`.
- Colors are CSS variables (`var:<code>` resolves to `var(--lcms-color-<code>)`); the band follows project styles automatically.
