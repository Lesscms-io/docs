# Hero Slider Widget (premium)

A full-width, multi-slide hero with layered entrance animations (label → heading → subheading → buttons), per-slide image/video/color backgrounds, overlay, autoplay and navigation. Uses nested element-group structure.

**Premium:** the widget is only available in the page builder for workspaces whose plan has the `premium_widgets` feature. The API returns it like any other widget.

## Widget Type

```
hero-slider
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"hero-slider"` |
| `uuid` | string | Unique widget identifier |
| `widget.items` | array | Slides (see below). With `config.source = "collection"` the slides are built server-side from the collection's entries (one per entry, `items` in the stored data are ignored) |
| `widget.config` | object | Slide source element group |
| `widget.config.source` | string | `"manual"` (slides from `items`) or `"collection"` (default: `"manual"`) |
| `widget.config.collection_code` | string\|null | Collection to read entries from |
| `widget.config.limit` | number | Max entries → slides (default: `5`) |
| `widget.config.order_by` | string\|null | Field code to sort by (`null` = collection default) |
| `widget.config.order_dir` | string | `"asc"`, `"desc"` (default: `"desc"`) |
| `widget.config.image_field` | string\|null | Entry field used as the slide background image |
| `widget.config.title_field` | string\|null | Entry field → slide title |
| `widget.config.subtitle_field` | string\|null | Entry field → slide subtitle |
| `widget.config.label_field` | string\|null | Entry field → slide label |
| `widget.config.button_show` | boolean | Render the primary button (links to the entry) (default: `true`) |
| `widget.config.button_text` | object | Multilingual text of that button |
| `widget.items[].label_show` | boolean | Whether the label/pill layer is rendered (default: `true`) |
| `widget.items[].label_style` | string | `"inherit"` (= `widget.label.style`), `"pill"`, `"outline"`, `"text"` (default: `"inherit"`) |
| `widget.items[].label` | object | Multilingual kicker/label `{ "en": "...", "pl": "..." }` |
| `widget.items[].title` | object | Multilingual heading |
| `widget.items[].subtitle` | object | Multilingual subheading |
| `widget.items[].background` | object | Slide background (see background fields) |
| `widget.items[].button_primary` | object | Primary button (see button fields) |
| `widget.items[].button_secondary` | object | Secondary button (see button fields) |
| `widget.items[].layout` | object | Per-slide layout override (see slide overrides) |
| `widget.items[].animation` | object | Per-slide animation override (see slide overrides) |
| `widget.layout` | object | Layout element group |
| `widget.layout.height` | string | `"sm"` (320px), `"md"` (440px), `"lg"` (560px), `"full"` (100vh) (default: `"lg"`) |
| `widget.layout.content_width` | string | Same presets as section content width: `"full"` (100%), `"wide"` (1400px), `"standard"` (1200px), `"narrow"` (960px), `"container"` (the project default section width, exposed as `--lcms-container-width`), `"custom"` (`custom_width`). The text block gets that width (+48px padding each side); the background always bleeds full width. Legacy `"normal"` renders as `"standard"`. (default: `"standard"`) |
| `widget.layout.custom_width` | number | Content max width in px for `content_width: "custom"` (default `1200`) |
| `widget.layout.gap` | number | Space between the layers (label / title / subtitle / buttons) in px (default `14`) |
| `widget.layout.position` | string | `"left"`, `"center"`, `"right"` (content over the full background), `"image-left"`, `"image-right"` (content on one half, media placement per `image_fill`) (default: `"center"`) |
| `widget.layout.image_fill` | string | Image layouts only: `"split"` (media as a half-width panel, the other half is `background.color`) or `"full"` (media across the whole slide, content still on its half) (default: `"split"`) |
| `widget.layout.text_align` | string | `"left"`, `"center"`, `"right"` (default: `"center"`) |
| `widget.layout.vertical_align` | string | `"top"`, `"center"`, `"bottom"` (default: `"center"`) |
| `widget.label` | object | Label element group |
| `widget.label.color` | string | Label text color (default: `"#ffffff"`) |
| `widget.label.color:hover` | string\|null | Label hover color |
| `widget.label.background` | string | Label background (default: `"var:primary"`) |
| `widget.label.background:hover` | string\|null | Label hover background |
| `widget.label.style` | string | `"pill"`, `"outline"`, `"text"` (default: `"pill"`) |
| `widget.label.font_family` | string | Font family name (one of the project fonts or a system font); empty = theme body font |
| `widget.label.margin_top` | number \| null | Space above the label in px; `null` = `layout.gap` |
| `widget.heading` | object | Heading element group |
| `widget.heading.color` | string | Heading color (default: `"#ffffff"`) |
| `widget.heading.color:hover` | string\|null | Heading hover color |
| `widget.heading.tag` | string | HTML tag `"h1"`, `"h2"`, `"h3"`, `"p"` (default: `"h1"`) |
| `widget.heading.size` | string | `"md"`, `"lg"`, `"xl"`, `"xxl"` (default: `"xl"`) |
| `widget.heading.font_family` | string | Font family name; empty = theme heading font |
| `widget.heading.margin_top` | number \| null | Space above the title in px; `null` = `layout.gap` |
| `widget.subheading` | object | Subheading element group |
| `widget.subheading.color` | string | Subheading color (default: `"rgba(255,255,255,0.85)"`) |
| `widget.subheading.color:hover` | string\|null | Subheading hover color |
| `widget.subheading.size` | string | `"sm"`, `"md"`, `"lg"` (default: `"md"`) |
| `widget.subheading.font_family` | string | Font family name; empty = theme body font |
| `widget.subheading.margin_top` | number \| null | Space above the subtitle in px; `null` = `layout.gap` |
| `widget.buttons` | object | Buttons element group (shared by both buttons) |
| `widget.buttons.primary_style` | string | Button style variant (default: `"primary"`) |
| `widget.buttons.secondary_style` | string | Button style variant (default: `"outline-light"`) |
| `widget.buttons.size` | string | `"sm"`, `"md"`, `"lg"` (default: `"lg"`) |
| `widget.buttons.border_radius` | string | `"none"`, `"sm"`, `"md"`, `"lg"`, `"pill"` (`"full"` = legacy alias of `"pill"`) (default: `"md"`) |
| `widget.buttons.gap` | number | Gap between buttons in px (default: `12`) |
| `widget.buttons.margin_top` | number \| null | Space above the buttons in px; `null` = `layout.gap` |
| `widget.overlay` | object | Overlay element group |
| `widget.overlay.color` | string | Overlay color (default: `"#000000"`) |
| `widget.overlay.color:hover` | string\|null | Overlay hover color |
| `widget.overlay.opacity` | number | 0–1 (default: `0.45`) |
| `widget.overlay.gradient` | string | `"none"`, `"bottom"`, `"top"`, `"left"`, `"right"` — gradient direction the color fades from (default: `"none"`) |
| `widget.animation` | object | Animation element group |
| `widget.animation.preset` | string | `"none"`, `"fade"`, `"stagger-up"`, `"stagger-left"`, `"zoom"`, `"reveal"` (default: `"stagger-up"`) |
| `widget.animation.duration` | number | Entrance duration in ms (default: `700`) |
| `widget.animation.stagger` | number | Delay between layers in ms (default: `140`) |
| `widget.animation.easing` | string | CSS timing function (default: `"ease-out"`) |
| `widget.animation.replay_on_slide` | boolean | Replay the layer entrance on every slide change; off = the slide moves as one piece (default: `true`) |
| `widget.animation.background_effect` | string | `"none"`, `"kenburns"`, `"parallax"` (default: `"kenburns"`) |
| `widget.animation.label` | string | Per-layer override (default: `"auto"` = from preset) |
| `widget.animation.title` | string | Per-layer override (default: `"auto"`) |
| `widget.animation.subtitle` | string | Per-layer override (default: `"auto"`) |
| `widget.animation.buttons` | string | Per-layer override (default: `"auto"`) |
| `widget.navigation` | object | Navigation element group |
| `widget.navigation.autoplay` | boolean | (default: `true`) |
| `widget.navigation.interval` | number | Slide time in ms (default: `6000`) |
| `widget.navigation.pause_on_hover` | boolean | (default: `true`) |
| `widget.navigation.loop` | boolean | (default: `true`) |
| `widget.navigation.arrows` | boolean | (default: `true`) |
| `widget.navigation.dots` | boolean | (default: `true`) |
| `widget.navigation.keyboard` | boolean | Arrow keys when focused (default: `true`) |
| `widget.navigation.swipe` | boolean | Touch swipe (default: `true`) |
| `widget.navigation.transition` | string | `"fade"`, `"slide"`, `"zoom"` (default: `"fade"`) |
| `widget.navigation.transition_duration` | number | Slide transition in ms (default: `700`) |
| `settings` | object | Common style settings (padding, margin, background, border…) |

### `widget.mobile` — phone overrides (below 768px)

| Property | Type | Description |
|----------|------|-------------|
| `widget.mobile` | object | Phone overrides (applied below 768px) |
| `widget.mobile.height` | string | `"auto"` (desktop height capped at 80vh), `"sm"` 320px, `"md"` 440px, `"lg"` 560px, `"full"` 100vh (default `"auto"`) |
| `widget.mobile.title_size` | string | `"auto"` (scaled desktop size), `"sm"` 26px, `"md"` 32px, `"lg"` 38px (default `"auto"`) |
| `widget.mobile.text_align` | string | `"auto"`, `"left"`, `"center"` (default `"auto"`) |
| `widget.mobile.hide_label` | boolean | Hide the label on phones (default `false`) |
| `widget.mobile.hide_subtitle` | boolean | Hide the subtitle on phones (default `false`) |
| `widget.mobile.hide_arrows` | boolean | Hide the arrows on phones (default `false`) |
| `widget.mobile.hide_dots` | boolean | Hide the dots on phones (default `false`) |
| `widget.mobile.padding` | number | Side padding of the text block on phones in px (default `24`) |

Rendered as root classes `hs--m-height-*`, `hs--m-title-*`, `hs--m-align-*`, `hs--m-hide-*` and `--hs-m-pad`, applied under the 768px media query.

### Background fields (`items[].background`)

| Property | Type | Description |
|----------|------|-------------|
| `type` | string | `"image"`, `"video"`, `"color"` (default: `"image"`) |
| `image` | string | Image URL (also the video poster) (default: `""`) |
| `video` | string | Video URL, MP4/WebM, played muted + looped (default: `""`) |
| `color` | string | Fallback/solid color (default: `"#1f2937"`) |
| `focal` | string | CSS `background-position` (default: `"center"`) |

### Slide overrides (`items[].layout` / `items[].animation`)

Every slide may deviate from the widget-level `layout` and `animation` groups. `"inherit"` (layout keys, animation preset) and `"auto"` (animation layers) mean "use the widget-level value". Height and timing (duration / stagger / easing) are widget-level only.

| Property | Type | Description |
|----------|------|-------------|
| `layout.position` | string | `"inherit"` or any `widget.layout.position` value |
| `layout.image_fill` | string | `"inherit"`, `"split"`, `"full"` |
| `layout.content_width` | string | `"inherit"` or any `widget.layout.content_width` value |
| `layout.text_align` | string | `"inherit"`, `"left"`, `"center"`, `"right"` |
| `layout.vertical_align` | string | `"inherit"`, `"top"`, `"center"`, `"bottom"` |
| `animation.preset` | string | `"inherit"` or any `widget.animation.preset` value |
| `animation.label` / `title` / `subtitle` / `buttons` | string | `"auto"` or a layer animation value |

Resolution: `effective = { ...widget.layout, ...slide.layout without "inherit" }`, same for animation (preset + pinned layers).

### Button fields (`button_primary` / `button_secondary`)

| Property | Type | Description |
|----------|------|-------------|
| `text` | object | Multilingual button text |
| `url` | string | Resolved URL (page/entry links are resolved server-side) (default: `"#"`) |
| `link_type` | string | `"custom"`, `"page"`, `"entry"`, `"route"` (default: `"custom"`) |
| `page_id` | string\|null | Page UUID for `page` links |
| `entry_id` | string\|null | Entry UUID for `entry` links |
| `collection_code` | string\|null | Collection code for `entry` links |
| `route_uuid` | string\|null | Route UUID |
| `target_blank` | boolean | Open in new tab (default: `false`) |
| `show` | boolean | Whether the button is rendered (primary default `true`, secondary default `false`) |
| `style` | string | `"inherit"` (= `widget.buttons.primary_style` / `secondary_style`) or a button style variant (default: `"inherit"`) |
| `size` | string | `"inherit"` (= `widget.buttons.size`), `"sm"`, `"md"`, `"lg"` (default: `"inherit"`) |
| `border_radius` | string | `"inherit"` (= `widget.buttons.border_radius`), `"none"`, `"sm"`, `"md"`, `"lg"`, `"pill"` (default: `"inherit"`) |
| `padding` | string | Custom padding in px (empty = the variant's default) |
| `icon` | string | Font Awesome class rendered inside the button (empty = none) |
| `icon_position` | string | `"left"`, `"right"` (default: `"left"`) |

### Layer animation values

`auto`, `none`, `fade`, `fade-up`, `fade-down`, `fade-left`, `fade-right`, `zoom-in`, `reveal`, `blur-in`.

Presets map to layers as: `stagger-up` = label fade-down, others fade-up; `stagger-left` = all fade-left; `zoom` = title & buttons zoom-in, label fade, subtitle fade-up; `reveal` = title reveal (clip-path wipe), label fade, rest fade-up.

## Example Response

```json
{
  "widget_type": "hero-slider",
  "uuid": "6f1d2c1e-0f4a-4c7c-9a1a-2b1d3e4f5a6b",
  "widget": {
    "items": [
      {
        "label_show": true,
        "label_style": "inherit",
        "label": { "pl": "Nowość", "en": "New" },
        "title": { "pl": "Kolekcja wiosna 2026", "en": "Spring 2026 collection" },
        "subtitle": { "pl": "Lekkie tkaniny, mocne kolory.", "en": "Light fabrics, bold colors." },
        "background": { "type": "image", "image": "https://cdn.example.com/hero-1.jpg", "video": "", "color": "#1f2937", "focal": "center" },
        "button_primary": { "text": { "pl": "Zobacz", "en": "Shop" }, "url": "/sklep", "link_type": "page", "page_id": "…", "entry_id": null, "collection_code": null, "route_uuid": null, "target_blank": false, "show": true, "style": "inherit", "size": "inherit", "border_radius": "inherit", "padding": "", "icon": "", "icon_position": "left" },
        "button_secondary": { "text": {}, "url": "#", "link_type": "custom", "page_id": null, "entry_id": null, "collection_code": null, "route_uuid": null, "target_blank": false, "show": false, "style": "inherit", "size": "inherit", "border_radius": "inherit", "padding": "", "icon": "", "icon_position": "left" },
        "layout": { "position": "image-right", "image_fill": "inherit", "content_width": "inherit", "text_align": "left", "vertical_align": "inherit" },
        "animation": { "preset": "inherit", "label": "auto", "title": "reveal", "subtitle": "auto", "buttons": "auto" }
      }
    ],
    "config": { "source": "manual", "collection_code": null, "limit": 5, "order_by": null, "order_dir": "desc", "image_field": null, "title_field": null, "subtitle_field": null, "label_field": null, "button_show": true, "button_text": {} },
    "layout": { "height": "lg", "content_width": "standard", "custom_width": 1200, "gap": 14, "position": "center", "image_fill": "split", "text_align": "center", "vertical_align": "center" },
    "label": { "color": "#ffffff", "color:hover": null, "background": "var:primary", "background:hover": null, "style": "pill", "font_family": "", "margin_top": null },
    "heading": { "color": "#ffffff", "color:hover": null, "tag": "h1", "size": "xl", "font_family": "Playfair Display", "margin_top": null },
    "subheading": { "color": "rgba(255,255,255,0.85)", "color:hover": null, "size": "md", "font_family": "", "margin_top": 8 },
    "buttons": { "primary_style": "primary", "secondary_style": "outline-light", "size": "lg", "border_radius": "md", "gap": 12, "margin_top": 24 },
    "overlay": { "color": "#000000", "color:hover": null, "opacity": 0.45, "gradient": "none" },
    "animation": { "preset": "stagger-up", "duration": 700, "stagger": 140, "easing": "ease-out", "replay_on_slide": true, "background_effect": "kenburns", "label": "auto", "title": "auto", "subtitle": "auto", "buttons": "auto" },
    "navigation": { "autoplay": true, "interval": 6000, "pause_on_hover": true, "loop": true, "arrows": true, "dots": true, "keyboard": true, "swipe": true, "transition": "fade", "transition_duration": 700 },
    "mobile": { "height": "auto", "title_size": "md", "text_align": "center", "hide_label": false, "hide_subtitle": false, "hide_arrows": true, "hide_dots": false, "padding": 24 }
  },
  "settings": { "padding_top": null, "padding_bottom": null }
}
```

## Rendering Notes

- Render only the first slide server-side (LCP); mount navigation, autoplay and transitions on the client.
- Entrance animations are CSS keyframes; stagger = `layerIndex × animation.stagger`. Re-key the content layer on slide change when `replay_on_slide` is `true`.
- Respect `prefers-reduced-motion`: skip entrance/transition animations and autoplay.
- Video backgrounds: `<video autoplay muted loop playsinline preload="metadata">` with `background.image` as `poster`.
- `layout.height = "full"` means `100vh`; cap at `80vh` on small screens.
- `image-left` / `image-right`: render the media in a 50% panel and the content on the other half over `background.color`; stack (media full-bleed behind content) below 768px.
