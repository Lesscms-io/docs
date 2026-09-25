# Mega Menu Widget (premium)

A navigation bar driven by a CMS menu (`/menus/{code}`), where every top-level item can open a **simple dropdown** (one column of links), a **mega panel** (child groups as columns, optional featured card) or be a plain link. The widget carries the look (bar, links, panel, CTA, mobile drawer) and per-item panel configuration; the structure and links come from the menu itself. Uses nested element-group structure.

**Premium:** the widget is only available in the page builder for workspaces whose plan has the `premium_widgets` feature. The API returns it like any other widget.

## Widget Type

```
mega-menu
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"mega-menu"` |
| `uuid` | string | Unique widget identifier |
| `widget.items` | array | Per-item panel configuration, keyed by the menu node id (see item fields). Nodes without an entry use the defaults. |
| `widget.config` | object | Source & behaviour element group |
| `widget.config.menu_code` | string\|null | Code of the CMS menu to render (`GET /menus/{code}`) |
| `widget.config.label_field` | string\|null | Menu field used as the link label (fallbacks: `title`, `name`, `label`, `text`, then the node `label`) |
| `widget.config.description_field` | string\|null | Menu field used as the link description (fallbacks: `description`, `subtitle`, `excerpt`, `summary`) |
| `widget.config.icon_field` | string\|null | Menu field with a Font Awesome class (fallback: `icon`) |
| `widget.config.image_field` | string\|null | Menu field with a thumbnail (fallbacks: `image`, `thumbnail`, `photo`, `cover`) |
| `widget.config.trigger` | string | `"hover"`, `"click"` (default: `"hover"`) |
| `widget.config.panel_default` | string | What a top-level item opens unless its own `panel` says otherwise: `"auto"` (mega when the children have children or a featured card, otherwise a simple dropdown), `"simple"` (plain dropdown list everywhere), `"mega"` (default: `"auto"`) |
| `widget.config.panel_width` | string | Width of **mega** panels: `"container"` (page container width), `"full"` (edge to edge), `"auto"` (fits content, anchored to the item) (default: `"container"`). Simple panels are always `"auto"` dropdowns unless the item overrides `width`. |
| `widget.config.mobile_breakpoint` | string | Collapse to hamburger below: `"sm"` (576px), `"md"` (768px), `"lg"` (992px), `"never"` (default: `"md"`) |
| `widget.config.mobile_mode` | string | `"offcanvas-right"`, `"offcanvas-left"`, `"fullscreen"`, `"dropdown"` (default: `"offcanvas-right"`) |
| `widget.logo` | object | Logo element group |
| `widget.logo.type` | string | `"image"`, `"text"`, `"none"` (default: `"image"`) |
| `widget.logo.light` | string\|null | Logo for light backgrounds — used by the light scheme; without schemes it is the default (top) logo |
| `widget.logo.dark` | string\|null | Logo for dark backgrounds — used by the dark scheme; without schemes it is shown in the scrolled state (when both are set) |
| `widget.logo.height` | number | Logo height in px (default: `40`) |
| `widget.logo.position` | string | Bar layout: `"left"` (logo, then items), `"center"` (items split in two halves around the logo), `"stacked"` (logo on its own row above centred items), `"right"` (items, then logo) (default: `"left"`) |
| `widget.logo.text` | object\|string | Text logo (multilingual or plain) |
| `widget.logo.font_size` | number | Text logo size in px (default: `20`) |
| `widget.logo.font_weight` | string | Text logo weight (default: `"700"`) |
| `widget.logo.font_family` | string | Text logo font family (project or system font); empty = theme heading font |
| `widget.logo.color` | string\|null | Text logo color |
| `widget.logo.color:scrolled` | string\|null | Text logo color when the section is scrolled (sticky) |
| `widget.bar` | object | Bar element group |
| `widget.bar.background` | string\|null | Bar background (`null` = transparent) |
| `widget.bar.background:scrolled` | string\|null | Bar background when scrolled |
| `widget.bar.height` | number | Bar height in px (default: `72`) |
| `widget.bar.width` | string | Width of the bar's content: `"container"` (centred, page container width) or `"full"` (default: `"container"`) |
| `widget.bar.align` | string | Items alignment: `"left"`, `"center"`, `"right"`, `"spread"` (default: `"left"`) |
| `widget.bar.shadow` | string | `"none"`, `"sm"`, `"md"` (default: `"none"`) |
| `widget.bar.divider` | boolean | Bottom border line (default: `false`) |
| `widget.bar.sticky` | boolean | Pin the bar to the top of the viewport once the page scrolls past it; the `*:scrolled` colours apply while pinned (default: `false`) |
| `widget.bar.sticky_mode` | string | `"always"` (stays visible) or `"scroll-up"` (pinned bar hides while scrolling down, reappears on scroll up) (default: `"always"`) |
| `widget.bar.overlay` | boolean | The bar floats over the content that follows (absolute, 0 height in the flow) — put the widget in a full-width, zero-padding section above a hero (default: `false`) |
| `widget.link` | object | Top-level link element group |
| `widget.link.color` | string | Link color (default: `"var:text"`) |
| `widget.link.color:hover` | string | Link hover/open color (default: `"var:primary"`) |
| `widget.link.color:scrolled` | string\|null | Link color when scrolled |
| `widget.link.background` | string\|null | Link background |
| `widget.link.background:hover` | string\|null | Link hover background (used by the `pill` hover animation) |
| `widget.link.font_size` | number | px (default: `15`) |
| `widget.link.font_weight` | string | (default: `"500"`) |
| `widget.link.font_family` | string | Font family of the bar items; empty = inherit |
| `widget.link.uppercase` | boolean | (default: `false`) |
| `widget.link.letter_spacing` | number | px (default: `0`) |
| `widget.link.hover_animation` | string | `"none"`, `"underline"`, `"pill"`, `"lift"` (default: `"underline"`) |
| `widget.link.border_radius` | string | Shape of the top link's hover/open highlight: `"none"`, `"sm"`, `"md"`, `"pill"` (default: `"pill"`) |
| `widget.link.chevron` | boolean | Show a chevron on items that open a panel (default: `true`) |
| `widget.link.gap` | number | Gap between items in px (default: `4`) |
| `widget.link.padding_x` | number | Horizontal item padding in px (default: `14`) |
| `widget.link.padding_y` | number | Vertical item padding in px (default: `10`) |
| `widget.panel` | object | Dropdown / mega panel element group |
| `widget.panel.background` | string | (default: `"#ffffff"`) |
| `widget.panel.color` | string | Base text color (default: `"var:text"`) |
| `widget.panel.heading_color` | string | Column heading color (default: `"var:dark"`) |
| `widget.panel.font_family` | string | Font family inside the panels; empty = inherit |
| `widget.panel.link_color` | string | Panel link color (default: `"var:text"`) |
| `widget.panel.link_color:hover` | string | Panel link hover color (default: `"var:primary"`) |
| `widget.panel.description_color` | string | (default: `"var:muted"`) |
| `widget.panel.icon_color` | string | (default: `"var:primary"`) |
| `widget.panel.border_radius` | string | `"none"`, `"sm"`, `"md"`, `"lg"` (default: `"md"`) |
| `widget.panel.shadow` | string | `"none"`, `"sm"`, `"md"`, `"lg"` (default: `"lg"`) |
| `widget.panel.border` | boolean | Draw a 1px border around the panel (default: `false`) |
| `widget.panel.border_color` | string | Panel border colour (default: `"var:border"`) |
| `widget.panel.padding` | number | Inner padding in px (default: `32`) |
| `widget.panel.columns_gap` | number | Gap between columns in px (default: `32`) |
| `widget.panel.animation` | string | Open animation: `"fade"`, `"fade-down"`, `"zoom"`, `"slide"` (default: `"fade-down"`) |
| `widget.panel.arrow` | boolean | Small arrow pointing at the bar (default: `true`) |
| `widget.panel.max_columns` | number | Cap for mega panel columns (default: `4`) |
| `widget.panel.link_padding_x` | number | Horizontal padding of a panel link in px (default: `10`) |
| `widget.panel.link_padding_y` | number | Vertical padding of a panel link in px (default: `8`) |
| `widget.panel.link_gap` | number | Gap between panel links in px (default: `2`) |
| `widget.panel.link_radius` | string | Panel link hover shape: `"none"`, `"sm"`, `"md"`, `"lg"` (default: `"md"`) |
| `widget.panel.link_font_size` | number | Panel link font size in px (default: `14`) |
| `widget.panel.link_background:hover` | string\|null | Panel link hover background (`null` = subtle grey) |
| `widget.cta` | object | Call-to-action button element group (see link fields) |
| `widget.cta.show` | boolean | (default: `false`) |
| `widget.cta.text` | object | Multilingual button text |
| `widget.cta.url` | string | Resolved URL (page/entry links are resolved server-side) (default: `"#"`) |
| `widget.cta.link_type` | string | `"custom"`, `"page"`, `"entry"`, `"route"` (default: `"custom"`) |
| `widget.cta.page_id` | string\|null | Page UUID for `page` links |
| `widget.cta.entry_id` | string\|null | Entry UUID for `entry` links |
| `widget.cta.collection_code` | string\|null | Collection code for `entry` links |
| `widget.cta.route_uuid` | string\|null | Route UUID |
| `widget.cta.target_blank` | boolean | Open in new tab (default: `false`) |
| `widget.cta.position` | string | `"left"` (before the items), `"right"` (default: `"right"`) |
| `widget.cta.style` | string | Button style variant (default: `"primary"`) |
| `widget.cta.size` | string | `"sm"`, `"md"`, `"lg"` (default: `"md"`) |
| `widget.cta.border_radius` | string | `"none"`, `"sm"`, `"md"`, `"lg"`, `"pill"` (default: `"md"`) |
| `widget.cta.icon` | string | Font Awesome class or `svg:<markup>` (empty = none) |
| `widget.cta.icon_position` | string | `"left"`, `"right"` (default: `"left"`) |
| `widget.mobile` | object | Mobile drawer element group |
| `widget.mobile.background` | string | (default: `"#ffffff"`) |
| `widget.mobile.color` | string | (default: `"var:text"`) |
| `widget.mobile.width` | number | Drawer width in px for off-canvas modes (default: `360`) |
| `widget.mobile.hamburger_color` | string\|null | Hamburger lines color (`null` = `link.color`) |
| `widget.mobile.animation` | string | `"slide"`, `"fade"` (default: `"slide"`) |
| `settings` | object | Common widget styles (padding, margin, background, …) |

### `widget.shop` — shop icons (projects linked to LessCommerce)

Rendered exactly like the [`ecommerce-icons`](ecommerce-icons.md) widget (search popover, cart badge + dropdown, account link).

| Property | Type | Description |
|----------|------|-------------|
| `widget.shop` | object | Shop icons group (search, account, cart) shown in the bar |
| `widget.shop.show` | boolean | Show the icon group in the bar (default: `false`) |
| `widget.shop.search` | boolean | Show the search icon (default `true`) |
| `widget.shop.account` | boolean | Show the account icon (default `true`) |
| `widget.shop.cart` | boolean | Show the cart icon with its counter (default `true`) |
| `widget.shop.search_icon` | string | Search icon class (default `fa-solid fa-magnifying-glass`) |
| `widget.shop.account_icon` | string | Account icon class (default `fa-solid fa-user`) |
| `widget.shop.cart_icon` | string | Cart icon class (default `fa-solid fa-bag-shopping`) |
| `widget.shop.custom_icon` | string | Optional fourth icon (custom link); empty = none |
| `widget.shop.custom_url` | string | URL of the custom icon |
| `widget.shop.custom_label` | multilingual | Accessible label of the custom icon |
| `widget.shop.size` | number | Icon size in px (default `18`) |
| `widget.shop.gap` | number | Gap between icons in px (default `10`) |
| `widget.shop.color` | color | Icon colour (default `var:text`) |
| `widget.shop.color:hover` | color | Icon hover colour (default `var:primary`) |
| `widget.shop.badge_background` | color | Cart count badge background (default `var:primary`) |
| `widget.shop.badge_color` | color | Cart count badge text (default `var:white`) |
| `widget.shop.highlight_shape` | string | `"circle"`, `"square"`, `"none"` — background disc behind each icon (default `"circle"`) |
| `widget.shop.highlight_background` | color | Disc background (default `var:background-alt`) |
| `widget.shop.highlight_color` | color | Icon colour inside the disc (default: inherit) |
| `widget.shop.highlight_padding` | number | Disc padding in px (default `10`) |
| `widget.shop.search_placeholder` | multilingual | Search popover placeholder |
| `widget.shop.mobile` | string | Below the mobile breakpoint: `"bar"` (stay next to the hamburger), `"dock"` (fixed bar at the bottom of the viewport), `"drawer-top"` / `"drawer-bottom"` (inside the off-canvas menu) (default `"bar"`) |

### `widget.schemes` — colour schemes (page top vs. scrolled)

Two named palettes for the bar and a choice of which applies at the page top and which once the sticky bar is scrolled (e.g. **dark** = transparent bar with white text over a hero, **light** = white bar with dark text after scrolling). When `enabled`, the palettes override `bar.background`, `link.color`, `logo.color`, `shop.color`, `mobile.hamburger_color` and their `*:scrolled` twins. The scrolled palette only shows when `bar.sticky` is on (there is no bar to recolour otherwise).

| Property | Type | Description |
|----------|------|-------------|
| `widget.schemes` | object | Colour schemes: palette at the page top and after scrolling |
| `widget.schemes.enabled` | boolean | Use the palettes instead of the per-group colours (default `false`) |
| `widget.schemes.top` | string | Palette at the page top: `"light"`, `"dark"` (default `"dark"`) |
| `widget.schemes.scrolled` | string | Palette once stuck after scrolling (default `"light"`) |
| `widget.schemes.light_background` | color | Light palette: bar background (default `#ffffff`) |
| `widget.schemes.light_text` | color | Light palette: links text (default `var:text`) |
| `widget.schemes.light_text_hover` | color \| null | Light palette: link hover colour (null → `link.color:hover`, then palette text) |
| `widget.schemes.light_logo` | color\|null | Light palette: text logo colour (`null` = same as text) |
| `widget.schemes.light_icons` | color\|null | Light palette: shop icons colour (`null` = same as text) |
| `widget.schemes.light_hamburger` | color\|null | Light palette: hamburger colour (`null` = same as text) |
| `widget.schemes.dark_background` | color | Dark palette: bar background (default `transparent`) |
| `widget.schemes.dark_text` | color | Dark palette: links text (default `#ffffff`) |
| `widget.schemes.dark_text_hover` | color \| null | Dark palette: link hover colour (null → `link.color:hover`, then palette text) |
| `widget.schemes.dark_logo` | color\|null | Dark palette: text logo colour (`null` = same as text) |
| `widget.schemes.dark_icons` | color\|null | Dark palette: shop icons colour (`null` = same as text) |
| `widget.schemes.dark_hamburger` | color\|null | Dark palette: hamburger colour (`null` = same as text) |

### Item fields (`widget.items[]`)

One entry per configured top-level menu node. Nodes with no entry behave as if all defaults were set.

| Property | Type | Description |
|----------|------|-------------|
| `widget.items[].id` | string | Menu node id (matches `id` in `/menus/{code}` content) |
| `widget.items[].panel` | string | `"auto"`, `"simple"`, `"mega"`, `"none"` (default: `"auto"`) |
| `widget.items[].columns` | number | Mega panel columns; `0` = one column per child group (default: `0`) |
| `widget.items[].width` | string | `"inherit"` (mega → `config.panel_width`, simple → `"auto"`), `"container"`, `"full"`, `"auto"` (default: `"inherit"`) |
| `widget.items[].show_descriptions` | boolean | Render `description_field` under links (default: `true`) |
| `widget.items[].show_icons` | boolean | Render `icon_field` next to links (default: `true`) |
| `widget.items[].show_images` | boolean | Render `image_field` thumbnails next to links (default: `false`) |
| `widget.items[].badge` | object | Small pill next to the top-level label (see badge fields) |
| `widget.items[].featured` | object | Featured card inside the panel, forces a mega panel (see featured fields) |

### Badge fields (`items[].badge`)

| Property | Type | Description |
|----------|------|-------------|
| `show` | boolean | (default: `false`) |
| `text` | object | Multilingual badge text |
| `color` | string | Badge background (default: `"var:primary"`) |

### Featured card fields (`items[].featured`)

| Property | Type | Description |
|----------|------|-------------|
| `show` | boolean | (default: `false`) |
| `position` | string | `"left"`, `"right"`, `"top"` (default: `"right"`) |
| `image` | string | Card background image URL |
| `title` | object | Multilingual card title |
| `text` | object | Multilingual card text |
| `button` | object | Card link (see link fields) plus `show` (boolean, toggles the button label; default `true`) |

### Panel type resolution (`panel = "auto"`)

- node has no children and no featured card → `none` (plain link)
- any child has its own children, or the featured card is on → `mega` (each direct child = one column heading, its children = the column links)
- otherwise → `simple` (direct children as one list of links)

An explicit `"simple"` / `"mega"` on a node without children and without a featured card still resolves to `none`.

### Link fields (`cta`, `items[].featured.button`)

| Property | Type | Description |
|----------|------|-------------|
| `text` | object | Multilingual text |
| `url` | string | Resolved URL (page/entry links are resolved server-side) (default: `"#"`) |
| `link_type` | string | `"custom"`, `"page"`, `"entry"`, `"route"` (default: `"custom"`) |
| `page_id` | string\|null | Page UUID for `page` links |
| `entry_id` | string\|null | Entry UUID for `entry` links |
| `collection_code` | string\|null | Collection code for `entry` links |
| `route_uuid` | string\|null | Route UUID |
| `target_blank` | boolean | Open in new tab (default: `false`) |

## Example Response

```json
{
  "widget_type": "mega-menu",
  "uuid": "2b7f1c9e-5d3a-4e8b-9f0c-1a2b3c4d5e6f",
  "widget": {
    "items": [
      {
        "id": "64f1a2b3c4d5e6f7a8b9c0d1",
        "panel": "auto",
        "columns": 0,
        "width": "inherit",
        "show_descriptions": true,
        "show_icons": true,
        "show_images": false,
        "badge": { "show": true, "text": { "pl": "Nowe", "en": "New" }, "color": "var:primary" },
        "featured": {
          "show": true,
          "position": "right",
          "image": "https://cdn.example.com/menu-promo.jpg",
          "title": { "pl": "Wiosenna kolekcja", "en": "Spring collection" },
          "text": { "pl": "Do -30% na wybrane modele.", "en": "Up to 30% off selected items." },
          "button": { "text": { "pl": "Zobacz", "en": "Shop" }, "url": "/sklep/wiosna", "link_type": "page", "page_id": "…", "entry_id": null, "collection_code": null, "route_uuid": null, "target_blank": false, "show": true }
        }
      }
    ],
    "config": { "menu_code": "main", "label_field": null, "description_field": "description", "icon_field": null, "image_field": null, "trigger": "hover", "panel_default": "auto", "panel_width": "container", "mobile_breakpoint": "md", "mobile_mode": "offcanvas-right" },
    "logo": { "type": "image", "light": "https://cdn.example.com/logo.svg", "dark": null, "height": 40, "position": "left", "text": "", "font_size": 20, "font_weight": "700", "font_family": "", "color": null, "color:scrolled": null },
    "bar": { "background": "#ffffff", "background:scrolled": null, "height": 72, "width": "container", "align": "left", "shadow": "sm", "divider": false, "sticky": true, "sticky_mode": "scroll-up", "overlay": false },
    "link": { "color": "var:text", "color:hover": "var:primary", "color:scrolled": null, "background": null, "background:hover": null, "font_size": 15, "font_weight": "500", "font_family": "", "uppercase": false, "letter_spacing": 0, "border_radius": "pill", "hover_animation": "underline", "chevron": true, "gap": 4, "padding_x": 14, "padding_y": 10 },
    "panel": { "background": "#ffffff", "color": "var:text", "heading_color": "var:dark", "font_family": "", "link_color": "var:text", "link_color:hover": "var:primary", "description_color": "var:muted", "icon_color": "var:primary", "border": false, "border_color": "var:border", "border_radius": "md", "shadow": "lg", "padding": 32, "columns_gap": 32, "animation": "fade-down", "arrow": true, "max_columns": 4, "link_padding_x": 10, "link_padding_y": 8, "link_gap": 2, "link_radius": "md", "link_font_size": 14, "link_background:hover": null },
    "cta": { "show": true, "text": { "pl": "Kontakt", "en": "Contact" }, "url": "/kontakt", "link_type": "page", "page_id": "…", "entry_id": null, "collection_code": null, "route_uuid": null, "target_blank": false, "position": "right", "style": "primary", "size": "md", "border_radius": "md", "icon": "", "icon_position": "left" },
    "mobile": { "background": "#ffffff", "color": "var:text", "width": 360, "hamburger_color": null, "animation": "slide" },
    "shop": { "show": true, "search": true, "account": true, "cart": true, "search_icon": "fa-solid fa-magnifying-glass", "account_icon": "fa-solid fa-user", "cart_icon": "fa-solid fa-bag-shopping", "custom_icon": "", "custom_url": "", "custom_label": {}, "size": 18, "gap": 10, "color": "var:text", "color:hover": "var:primary", "badge_background": "var:primary", "badge_color": "var:white", "highlight_shape": "circle", "highlight_background": "var:background-alt", "highlight_color": null, "highlight_padding": 10, "search_placeholder": { "pl": "Szukaj…" }, "mobile": "dock" },
    "schemes": { "enabled": true, "top": "dark", "scrolled": "light", "light_background": "#ffffff", "light_text": "var:text", "light_text_hover": null, "light_logo": null, "light_icons": null, "light_hamburger": null, "dark_background": "transparent", "dark_text": "#ffffff", "dark_text_hover": null, "dark_logo": null, "dark_icons": null, "dark_hamburger": null }
  },
  "settings": { "padding_top": 0, "padding_right": 0, "padding_bottom": 0, "padding_left": 0 }
}
```

## Rendering Notes

- Fetch the menu with `GET /menus/{config.menu_code}` and render **only nodes with `metadata.is_public !== false`**. Render the nav server-side (crawlable links, no layout jump).
- Each top-level `<li>` holds its `<a>` and its panel; the panel is positioned under the bar (`container` / `full`) or under the item (`auto`). Close on `Escape`, on outside click and when the pointer leaves the item + panel (with a ~120 ms grace delay for hover).
- `trigger = "hover"`: the label stays a real link; the chevron toggles the panel on touch devices. `trigger = "click"`: the label toggles the panel and does not navigate.
- Below `config.mobile_breakpoint` replace the items and the CTA with a hamburger; the drawer lists all levels as an accordion, the featured card as an image tile, the CTA at the bottom. Lock body scroll while an off-canvas / fullscreen drawer is open.
- With `schemes.enabled` the logo image follows the active scheme (`schemes.top` at the page top, `schemes.scrolled` once stuck): light → `logo.light`, dark → `logo.dark`. Otherwise `*:scrolled` colours and `logo.dark` apply when the containing section is sticky and scrolled, or when `bar.sticky` is on and the bar is pinned (`is-scrolled` class in the reference renderer).
- `bar.sticky`: once the widget's top edge leaves the viewport, render the bar `position: fixed; top: 0` and keep a spacer of the bar's height in place of it (no layout jump). Order of the bar's children follows `logo.position` / `cta.position`: CTA left → before the items, CTA right → after them; a centred logo sits between the two halves of the items.
- The panel's column count is `min(items[].columns || number of child groups, panel.max_columns)`.
