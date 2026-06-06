# Nav Bar Widget

Slot-based header. Each slot (`left` / `center` / `right` by default) carries a list of typed items: `logo`, `menu`, `cta`, `link`, `icon`, `text`, `search`, `divider`. Past the configured `hamburger.breakpoint`, slots marked `move-to-drawer` get pulled into a side / overlay drawer toggled from the slot named in `hamburger.toggle_slot`.

## Widget Type

```
nav-bar
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"nav-bar"` |
| `uuid` | string | Unique widget identifier |
| `widget.slots` | array | Ordered list of slot definitions |
| `widget.slots[].id` | string | Slot id (`"left"` / `"center"` / `"right"` by default) |
| `widget.slots[].width` | string | Slot width sizing: `"auto"` \| `"grow"` \| `"<N>%"` \| `"<N>px"` |
| `widget.slots[].align` | string | Inline alignment of slot content: `"start"` \| `"center"` \| `"end"` |
| `widget.slots[].mobile_target` | string | What happens to the slot past `hamburger.breakpoint`: `"move-to-drawer"` \| `"keep-inline"` \| `"hide"` |
| `widget.slots[].items` | array | Typed items inside the slot |
| `widget.hamburger` | object | Hamburger / drawer config |
| `widget.hamburger.breakpoint` | string | When to switch to hamburger: `"never"` \| `"tablet"` \| `"mobile"` (default `"mobile"`) |
| `widget.hamburger.position` | string | Drawer placement: `"right"` \| `"left"` \| `"full-overlay"` \| `"push-content"` |
| `widget.hamburger.width` | number | Drawer width in px (ignored for `full-overlay`) |
| `widget.hamburger.bg` | string\|null | Drawer background (color variable or hex) |
| `widget.hamburger.text_color` | string\|null | Drawer text color |
| `widget.hamburger.item_gap` | number | Gap between items inside drawer (px) |
| `widget.hamburger.padding` | number | Drawer padding (px) |
| `widget.hamburger.cta_style` | string | CTA layout inside drawer: `"full-width"` \| `"inline"` \| `"pinned-to-bottom"` |
| `widget.hamburger.toggle_icon` | string | Font Awesome class for the open button |
| `widget.hamburger.close_icon` | string | Font Awesome class for the close button |
| `widget.hamburger.toggle_slot` | string | Which slot renders the hamburger toggle button: `"left"` \| `"center"` \| `"right"` |
| `settings.*` | mixed | Common widget settings — padding, margin, background, sticky, scrolled state, responsive overrides |

## Item Types

Each item has shape `{ type, config }`. Allowed `type` values and their `config` schema:

### `logo`

| Property | Type | Description |
|----------|------|-------------|
| `image` | string\|null | Image URL (SVG, PNG, WebP, etc.) |
| `height` | number | Image height in px (width auto) |
| `link` | string\|null | URL to navigate to on click (typically `/`) |

### `menu`

| Property | Type | Description |
|----------|------|-------------|
| `menu_code` | string\|null | Code of the menu defined in LessCMS |
| `layout` | string | `"horizontal"` (default) — vertical layout used automatically in drawer |
| `items_gap` | number | Gap between menu items (px) |
| `color` | string\|null | Link color |
| `color:hover` | string\|null | Link color on hover |

### `cta`

| Property | Type | Description |
|----------|------|-------------|
| `text` | string | Button label |
| `link` | string\|null | URL |
| `target_blank` | boolean | Open in new tab |
| `style` | string | `"info"` (default) / `"success"` / `"danger"` / `"dark"` / `"light"` / `"outline"` / `"link"` |
| `size` | string | `"sm"` / `"md"` (default) / `"lg"` |
| `border_radius` | string | `"none"` / `"sm"` / `"md"` (default) / `"lg"` / `"full"` |
| `icon` | string\|null | Font Awesome class |
| `icon_position` | string | `"left"` (default) / `"right"` |

### `link`

| Property | Type | Description |
|----------|------|-------------|
| `text` | string | Link label |
| `link` | string\|null | URL |
| `target_blank` | boolean | Open in new tab |
| `color` | string\|null | Text color |
| `color:hover` | string\|null | Text color on hover |

### `icon`

| Property | Type | Description |
|----------|------|-------------|
| `icon` | string\|null | Font Awesome class |
| `size` | number | Icon size in px (default 18) |
| `link` | string\|null | URL on click |
| `color` | string\|null | Icon color |

### `text`

| Property | Type | Description |
|----------|------|-------------|
| `html` | string | Arbitrary inline HTML / text |

### `search`

| Property | Type | Description |
|----------|------|-------------|
| `url` | string | Search results page URL (default `/search`) |
| `icon` | string | Font Awesome class (default `fa-solid fa-magnifying-glass`) |
| `size` | number | Icon size in px |

### `divider`

| Property | Type | Description |
|----------|------|-------------|
| `thickness` | number | Vertical line thickness (px) |
| `height` | number | Vertical line height (px) |
| `color` | string\|null | Line color |

## Example Response

```json
{
  "widget_type": "nav-bar",
  "uuid": "ab2c-...-9f",
  "widget": {
    "slots": [
      {
        "id": "left",
        "width": "auto",
        "align": "start",
        "mobile_target": "move-to-drawer",
        "items": [
          { "type": "link", "config": { "text": "O autorce", "link": "/o-autorce" } },
          { "type": "link", "config": { "text": "Metody", "link": "/metody" } }
        ]
      },
      {
        "id": "center",
        "width": "auto",
        "align": "center",
        "mobile_target": "keep-inline",
        "items": [
          { "type": "logo", "config": { "image": "https://cdn.../logo.svg", "height": 40, "link": "/" } }
        ]
      },
      {
        "id": "right",
        "width": "auto",
        "align": "end",
        "mobile_target": "move-to-drawer",
        "items": [
          { "type": "cta", "config": { "text": "Zapraszam do kontaktu", "link": "/kontakt", "style": "info", "size": "md" } }
        ]
      }
    ],
    "hamburger": {
      "breakpoint": "mobile",
      "position": "right",
      "width": 320,
      "item_gap": 16,
      "padding": 24,
      "cta_style": "full-width",
      "toggle_icon": "fa-solid fa-bars",
      "close_icon": "fa-solid fa-xmark",
      "toggle_slot": "right"
    }
  },
  "settings": {
    "padding_top": 16,
    "padding_right": 24,
    "padding_bottom": 16,
    "padding_left": 24,
    "sticky": true,
    "sticky_top": 0
  }
}
```

## Notes

- A single `menu`-type item is the source of truth for items both inline and in the drawer — no need to duplicate menu data per breakpoint. If you need *different* items per breakpoint, place two `menu` items in the slot and toggle their visibility via responsive overrides at the parent widget level.
- `widget.slots` is intentionally array-shaped (not `{left, center, right}` object) so you can add a fourth slot later without a migration.
- The renderer uses CSS `flex` with `justify-content: space-between` — slot widths of `auto` snap to their content, `grow` pushes the slot to consume free space.
