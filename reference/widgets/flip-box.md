# Flip Box Widget (premium)

A two-sided card: the front (icon or image, title, text) reveals the back
(title, text, button) on hover or click with a 3D flip, slide, zoom or fade.
Requires the `premium_widgets` plan feature.

## Widget Type

```
flip-box
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"flip-box"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget properties |
| `widget.front` | object | Front side |
| `widget.front.icon` | string | Font Awesome class (shown when no image) |
| `widget.front.image` | string | Image URL shown above the title |
| `widget.front.title` | object | Multilingual title |
| `widget.front.text` | object | Multilingual text (newlines preserved) |
| `widget.front.background` | string | Background color (`var:code` or hex) |
| `widget.front.background_image` | string | Background image URL (cover) |
| `widget.front.color` | string | Text color |
| `widget.front.align` | string | `"left"`, `"center"`, `"right"` |
| `widget.back` | object | Back side |
| `widget.back.icon` | string | Font Awesome class (shown when no image) |
| `widget.back.image` | string | Image URL shown above the title |
| `widget.back.title` | object | Multilingual title |
| `widget.back.text` | object | Multilingual text (newlines preserved) |
| `widget.back.background` | string | Background color (`var:code` or hex) |
| `widget.back.background_image` | string | Background image URL (cover) |
| `widget.back.color` | string | Text color |
| `widget.back.align` | string | `"left"`, `"center"`, `"right"` |
| `widget.button` | object | Button on the back side |
| `widget.button.show` | boolean | Render the button (default `true`) |
| `widget.button.text` | object | Multilingual button label |
| `widget.button.style` | string | Button style (`primary`, `light`, `outline-light`, …) |
| `widget.button.size` | string | `"sm"`, `"md"`, `"lg"` |
| `widget.button.border_radius` | string | `"none"`, `"sm"`, `"md"`, `"lg"`, `"pill"` |
| `widget.button.url` | string | Resolved URL (page / entry links resolved server-side) |
| `widget.button.link_type` | string | `"custom"`, `"page"`, `"entry"`, `"route"` |
| `widget.button.page_id` | string\|null | Page UUID for page links |
| `widget.button.collection_code` | string\|null | Collection code for entry links |
| `widget.button.entry_id` | string\|null | Entry UUID for entry links |
| `widget.button.route_uuid` | string\|null | Route UUID for route links |
| `widget.button.target_blank` | boolean | Open in a new tab |
| `widget.config` | object | Effect and sizing |
| `widget.config.effect` | string | `"flip-h"`, `"flip-v"`, `"slide-up"`, `"slide-left"`, `"zoom"`, `"fade"` |
| `widget.config.trigger` | string | `"hover"` or `"click"` (touch screens always flip on tap) |
| `widget.config.height` | number | Card height in px (default `320`) |
| `widget.config.duration` | number | Transition duration in ms (default `600`) |
| `widget.config.radius` | number | Corner radius in px (default `12`) |
| `widget.config.padding` | number | Inner padding in px (default `32`) |
| `widget.config.icon_size` | number | Icon size in px (default `48`) |
| `widget.config.title_size` | number | Title font size in px (default `24`) |
| `widget.config.overlay` | number | Dark overlay opacity over background images, `0`–`1` (default `0.35`) |
| `widget.config.font_family` | string | Font family (empty = inherit) |
| `settings` | object | Style settings (padding, background, border, hover transforms) |

## Example Response

```json
{
  "widget_type": "flip-box",
  "uuid": "flip-box-123",
  "widget": {
    "front": {
      "icon": "fa-solid fa-rocket", "image": "",
      "title": { "en": "Fast delivery", "pl": "Szybka dostawa" },
      "text": { "en": "Ships within 24h", "pl": "Wysyłka w 24h" },
      "background": "var:primary", "background_image": "", "color": "#ffffff", "align": "center"
    },
    "back": {
      "icon": "", "image": "",
      "title": { "en": "How it works", "pl": "Jak to działa" },
      "text": { "en": "Orders placed before 2pm leave the same day.", "pl": "Zamówienia do 14:00 wysyłamy tego samego dnia." },
      "background": "var:dark", "background_image": "", "color": "#ffffff", "align": "center"
    },
    "button": {
      "show": true, "text": { "en": "Learn more", "pl": "Dowiedz się więcej" },
      "style": "light", "size": "md", "border_radius": "md",
      "url": "/delivery", "link_type": "page", "page_id": "…", "collection_code": null, "entry_id": null, "route_uuid": null, "target_blank": false
    },
    "config": { "effect": "flip-h", "trigger": "hover", "height": 320, "duration": 600, "radius": 12, "padding": 32, "icon_size": 48, "title_size": 24, "overlay": 0.35, "font_family": "" }
  },
  "settings": {}
}
```

## Rendering Notes

- Render both sides in the DOM (SEO); position them absolutely inside a container with `perspective`, and use `backface-visibility: hidden` for the flip effects.
- `trigger: "hover"` uses `:hover` only under `@media (hover: hover)`; on touch devices toggle on tap.
- Prefer this widget inside a wrapper (`wrappable`) to build 2–4 column feature grids.
