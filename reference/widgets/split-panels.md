# Split Panels Widget

Hero "tryptyk": 2-5 vertical panels at full screen height. Each panel has a background photo with a gradient, an outlined number (fills on hover), a title, a text (revealed on hover) and a "more" link whose line grows on hover. Hovering a panel widens it and dims the others. An optional round cursor follows the mouse over the panels. On phones (<=768px) the panels stack (~72vh each) and the text is always visible.

Place the widget in a full-width section (`content_width: 100%`, no section padding), the same way as `hero-slider`.

## Widget Type

```
split-panels
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"split-panels"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget data (element groups) |
| `widget.items` | array | Panels, max 5 (see below) |
| `widget.items[].image` | string | Background image URL (empty = no photo) |
| `widget.items[].alt` | object | Multilingual image alt text |
| `widget.items[].number` | string | Number shown on the panel; auto `01`, `02`, ... when empty |
| `widget.items[].title` | object | Multilingual title |
| `widget.items[].text` | object | Multilingual text, shown on hover (always on mobile) |
| `widget.items[].link` | object | Panel link: `url` (resolved server-side from page/entry/route), `link_type`, `page_id`, `entry_id`, `collection_code`, `route_uuid`, `target_blank` |
| `widget.items[].link_label` | object | Multilingual "more" link text; the link line is shown only when set |
| `widget.config` | object | Layout and behaviour options (see below) |
| `widget.config.height` | string | `"full"` (100vh, default) or `"custom"` |
| `widget.config.custom_height` | number | Height value for `"custom"` (default: 80) |
| `widget.config.custom_unit` | string | `"vh"` (default) or `"px"` |
| `widget.config.expand_on_hover` | boolean | Hovered panel widens, others narrow (default: true) |
| `widget.config.dim_others` | boolean | Other panels turn grayscale and darker on hover (default: true) |
| `widget.config.custom_cursor` | boolean | Round cursor follows the mouse over panels, desktop only (default: false) |
| `widget.config.cursor_label` | object | Multilingual cursor label (default text: "Zobacz") |
| `widget.config.intro_animation` | boolean | Panels rise from the bottom, staggered (default: true) |
| `widget.config.overlay_strength` | number | Gradient darkness at the bottom, 0-100 (default: 85) |
| `widget.number` | object | Number element group |
| `widget.number.style` | string | `"outline"` (default, fills on hover) or `"fill"` |
| `widget.number.color` | string\|null | Number color |
| `widget.number.color:hover` | string\|null | Number color on panel hover |
| `widget.number.text_style` | string | Project text style code (applied as `lcms-ts-<code>`) |
| `widget.title` | object | Title element group |
| `widget.title.color` | string\|null | Title color |
| `widget.title.tag` | string | `"h2"` (default) or `"h3"` |
| `widget.text` | object | Text element group |
| `widget.text.color` | string\|null | Text color |
| `widget.link` | object | Link element group |
| `widget.link.color` | string\|null | "More" link color |
| `widget.link.color:hover` | string\|null | "More" link color on panel hover |
| `widget.cursor` | object | Cursor element group |
| `widget.cursor.background` | string\|null | Cursor background color (default: `var:primary`) |
| `widget.cursor.color` | string\|null | Cursor text color (default: `var:dark`) |
| `settings` | object | [Shared widget settings](shared-settings.md) |

Colors use the `var:<code>` notation or a CSS color; `null` means "not set" (the theme default applies).

## Example Response

```json
{
  "widget_type": "split-panels",
  "uuid": "sp-001",
  "widget": {
    "items": [
      {
        "image": "https://cdn.example.com/filar-1.jpg",
        "alt": { "pl": "Inwestycja budowlana" },
        "number": "01",
        "title": { "pl": "Kompleksowa realizacja inwestycji budowlanych" },
        "text": { "pl": "Prowadzimy inwestycje od przygotowania po wykończenie." },
        "link": { "url": "/uslugi/realizacja", "link_type": "page", "page_id": "a1b2", "target_blank": false },
        "link_label": { "pl": "Zobacz więcej" }
      },
      {
        "image": "https://cdn.example.com/biuro-2.jpg",
        "alt": { "pl": "Wnętrze biurowe" },
        "number": "02",
        "title": { "pl": "Remonty, modernizacje i wykończenia" },
        "text": { "pl": "Remonty w obiektach mieszkalnych i komercyjnych." },
        "link": { "url": "/uslugi/remonty", "link_type": "page", "page_id": "c3d4", "target_blank": false },
        "link_label": { "pl": "Zobacz więcej" }
      }
    ],
    "config": {
      "height": "full",
      "custom_height": 80,
      "custom_unit": "vh",
      "expand_on_hover": true,
      "dim_others": true,
      "custom_cursor": true,
      "cursor_label": { "pl": "Zobacz" },
      "intro_animation": true,
      "overlay_strength": 85
    },
    "number": { "style": "outline", "color": "var:white", "color:hover": "var:primary", "text_style": "" },
    "title": { "color": "var:white", "tag": "h2" },
    "text": { "color": null },
    "link": { "color": "var:white", "color:hover": "var:primary" },
    "cursor": { "background": "var:primary", "color": "var:dark" }
  },
  "settings": {}
}
```

## Notes

- Page and entry links are resolved on the server; `items[].link.url` is ready to use.
- Numbers are automatic (`01`, `02`, ...) when `items[].number` is empty.
- Maximum 5 panels; extra items are ignored.
- Reduced motion (`prefers-reduced-motion: reduce`) disables the intro animation and transitions.
