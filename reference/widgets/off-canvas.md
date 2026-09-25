# Off-canvas Widget (premium)

A trigger (button, icon or text link) that opens a drawer from any edge of
the screen with a CMS menu and / or rich text — mobile navigation, contact
box, filters. Requires the `premium_widgets` plan feature.

## Widget Type

```
off-canvas
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"off-canvas"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget properties |
| `widget.trigger` | object | What opens the drawer |
| `widget.trigger.type` | string | `"button"`, `"icon"` (icon only), `"text"` (underlined link) |
| `widget.trigger.text` | object | Multilingual label |
| `widget.trigger.icon` | string | Font Awesome class |
| `widget.trigger.icon_position` | string | `"left"` or `"right"` of the label |
| `widget.trigger.style` | string | Button style (`primary`, `outline-primary`, …) — `button` type |
| `widget.trigger.size` | string | `"sm"`, `"md"`, `"lg"` — `button` type |
| `widget.trigger.border_radius` | string | `"none"`, `"sm"`, `"md"`, `"lg"`, `"pill"` — `button` type |
| `widget.trigger.align` | string | `"left"`, `"center"`, `"right"` |
| `widget.trigger.color` | string | Color for `icon` / `text` triggers |
| `widget.panel` | object | The drawer |
| `widget.panel.side` | string | `"right"`, `"left"`, `"top"`, `"bottom"` |
| `widget.panel.width` | number | Drawer width in px for left / right (default `380`) |
| `widget.panel.title` | object | Multilingual heading |
| `widget.panel.html` | object | Multilingual HTML content |
| `widget.panel.menu_code` | string | CMS menu code rendered as a link list (empty = none) — fetch via `/menus/{code}` |
| `widget.panel.menu_position` | string | `"above"` or `"below"` the HTML content |
| `widget.panel.background` | string | Drawer background |
| `widget.panel.color` | string | Drawer text color |
| `widget.panel.padding` | number | Inner padding in px |
| `widget.panel.radius` | number | Corner radius in px |
| `widget.panel.close` | boolean | Show the × close button |
| `widget.panel.link_size` | number | Menu link font size in px |
| `widget.backdrop` | object | Backdrop |
| `widget.backdrop.color` | string | Backdrop color |
| `widget.backdrop.blur` | number | Backdrop blur in px |
| `widget.backdrop.close_on_click` | boolean | Clicking the backdrop closes the drawer |
| `widget.animation` | object | Open / close animation |
| `widget.animation.type` | string | `"slide"`, `"fade"`, `"scale"` |
| `widget.animation.duration` | number | Duration in ms (default `350`) |
| `settings` | object | Style settings (padding, background, border, hover transforms) |

## Example Response

```json
{
  "widget_type": "off-canvas",
  "uuid": "off-canvas-123",
  "widget": {
    "trigger": { "type": "icon", "text": { "en": "Menu", "pl": "Menu" }, "icon": "fa-solid fa-bars", "icon_position": "left", "style": "primary", "size": "md", "border_radius": "md", "align": "right", "color": "var:dark" },
    "panel": {
      "side": "right", "width": 380,
      "title": { "en": "Menu", "pl": "Menu" },
      "html": { "en": "<p>Call us: <a href=\"tel:+48123456789\">+48 123 456 789</a></p>", "pl": "<p>Zadzwoń: <a href=\"tel:+48123456789\">+48 123 456 789</a></p>" },
      "menu_code": "main", "menu_position": "above",
      "background": "#ffffff", "color": "var:dark", "padding": 28, "radius": 0, "close": true, "link_size": 16
    },
    "backdrop": { "color": "var:dark:60", "blur": 4, "close_on_click": true },
    "animation": { "type": "slide", "duration": 350 }
  },
  "settings": {}
}
```

## Rendering Notes

- Render the drawer at the end of `body` (teleport) with `position: fixed`; lock body scroll while open; close on ESC and (optionally) backdrop click; return focus to the trigger.
- `panel.menu_code` → `GET /menus/{code}`; render `items[].label` / `metadata.url` and one level of `children`.
- The drawer is `role="dialog" aria-modal="true"`; the trigger carries `aria-expanded`.
