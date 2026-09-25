# AI Frontend Generation

This guide explains how to use AI tools (Claude, ChatGPT, Cursor) to generate a universal frontend engine that consumes the LessCMS API.

## Concept

Instead of hardcoding pages, the AI should generate a **universal rendering engine** that:

1. Fetches routes from `/routes` API
2. Dynamically renders any page based on URL matching
3. Renders all widget types from the API response
4. Applies styles from `/config` API
5. Uses a single CSS file with `lcms-` prefixed classes

This approach creates a frontend that automatically adapts to any LessCMS project without code changes.

## Key API Endpoints

The AI needs to know about these endpoints:

| Endpoint | Purpose |
|----------|---------|
| `GET /routes` | All routes with URL patterns and targets |
| `GET /config` | Project configuration (styles, colors, fonts) |
| `GET /pages/{code}` | Page content with sections and widgets |
| `GET /collections/{code}` | Collection schema |
| `GET /collections/{code}/entries` | Collection entries |
| `GET /menus/{code}` | Navigation menu items |

## The Prompt

Use this prompt to generate a universal LessCMS frontend engine:

````
Generate a universal frontend engine for LessCMS headless CMS.

## API Base
https://api.lesscms.io/api/{project-code}

Authentication: X-API-Key header

## Key Endpoints

### GET /routes
Returns all routes with URL patterns:
```json
{
  "routes": [
    {
      "pattern": "/",
      "type": "page",
      "target": "home"
    },
    {
      "pattern": "/about",
      "type": "page",
      "target": "about"
    },
    {
      "pattern": "/blog",
      "type": "collection",
      "target": "blog",
      "view": "list"
    },
    {
      "pattern": "/blog/:slug",
      "type": "collection",
      "target": "blog",
      "view": "detail"
    }
  ]
}
```

### GET /config
Returns project configuration:
```json
{
  "languages": ["pl", "en"],
  "default_language": "pl",
  "styles": {
    "primary_color": "#007BFF",
    "secondary_color": "#6c757d",
    "font_heading": "Playfair Display",
    "font_body": "Inter",
    "border_radius": 8
  }
}
```

### GET /pages/{code}
Returns page with sections and widgets (see Widgets documentation).

## Architecture Requirements

1. **Universal Router**
   - Fetch routes from /routes on app init
   - Match current URL to route pattern
   - Handle dynamic segments (:slug, :id)
   - Render appropriate view (page/collection list/collection detail)

2. **Widget Renderer**
   - Single component that renders ANY widget type
   - Switch/map based on `widget_type`
   - Pass `widget.widget` (all widget data, flat object) and `widget.settings` (styles) to specific renderers
   - Handle unknown widget types gracefully

3. **CSS Requirements** (CRITICAL)
   - All CSS in a single file: `lcms-styles.css`
   - ALL classes prefixed with `lcms-` (e.g., `lcms-section`, `lcms-widget-heading`)
   - CSS variables for config values (--lcms-primary-color, --lcms-font-heading)
   - This allows LessCMS to inject styles into the page builder preview

4. **Section/Column Renderer**
   - Render sections with columns
   - Apply section settings (background, padding, etc.)
   - Apply column width percentages
   - Responsive stacking on mobile

## Section, Column and Widget Settings Structure

The page builder uses a three-level structure: **Sections** contain **Columns**, and Columns contain **Widgets**. Each level has its own `settings` object with specific properties that control styling and behavior.

### Section Settings

Sections are the top-level containers. Each section has:

```json
{
  "uuid": "section-uuid",
  "grid_type": "2-columns",
  "settings": {
    // Size & Layout
    "fullHeight": false,
    "sectionHeight": null,
    "contentWidth": "1200px",
    "customWidth": 1200,
    "columnGap": 20,

    // Background
    "backgroundColor": "#ffffff",
    "backgroundOpacity": 100,
    "backgroundImage": "https://...",
    "backgroundSize": "cover", // "cover" | "contain" | "auto" | raw CSS like "200px auto" or "30% auto"
    "backgroundPosition": "center center",
    "backgroundImageOpacity": 100,
    "backgroundAttachment": "scroll", // "fixed" = image pinned to the viewport, content scrolls over it (parallax-like); with a gradient use `scroll, fixed`
    "backgroundImageSource": "static",
    "backgroundImageField": "",
    "backgroundVideoUrl": "", // optional: uploaded video URL (mp4/webm); plays muted+looped under content, hidden on mobile. backgroundImage acts as poster/fallback.

    // Gradient
    "useGradient": false,
    "gradientType": "linear",
    "gradientAngle": 180,
    "gradientColorStart": "#667eea",
    "gradientColorEnd": "#764ba2",

    // Spacing (in pixels)
    "paddingTop": 40,
    "paddingRight": 20,
    "paddingBottom": 40,
    "paddingLeft": 20,
    "marginTop": 0,
    "marginRight": 0,
    "marginBottom": 0,
    "marginLeft": 0,

    // Border
    "borderRadius": 0,
    "borderWidth": 0,
    "borderColor": "#000000",
    "borderStyle": "solid",
    "boxShadow": "",

    // Responsive
    "stackOnTablet": false,
    "stackOnMobile": true,
    "hidden": false,
    "responsive": {
      "tablet": { "hidden": false, "paddingTop": 20 },
      "mobile": { "hidden": false, "paddingTop": 10 }
    },

    // Link (clickable section)
    "link": {
      "enabled": false,
      "type": "custom",
      "url": "",
      "page_id": null,
      "collection_code": null,
      "entry_id": null,
      "route_uuid": null,
      "target_blank": false
    },

    // Sticky
    "sticky": false,
    "stickyTop": 0,
    "stickyZIndex": 100,
    "scrolledBg": "#ffffff",
    "scrolledShadow": "md",

    // Advanced
    "cssId": "",
    "cssClass": ""
  },
  "columns": [...]
}
```

#### Section Settings Reference

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `fullHeight` | boolean | `false` | Section takes 100vh height |
| `sectionHeight` | number/null | `null` | Fixed height in pixels |
| `contentWidth` | string | `"100%"` | Inner content width ("100%", "1400px", "1200px", "960px", "custom") |
| `customWidth` | number | `1200` | Custom width when contentWidth is "custom" |
| `customWidthUnit` | string | `"px"` | Unit of `customWidth`: `"px"`, `"%"`, `"vw"` — the API emits `content_width` with the unit (e.g. `"80%"`) |
| `contentAlign` | string | `"center"` | Placement of a narrower content box: `"left"`, `"center"`, `"right"` (margin auto on the free side) |
| `customMaxWidth` | number/null | `null` | px cap for a `%`/`vw` custom width — API emits `content_max_width`, renderer uses `max-width: min(80%, 1080px)` |
| `backgroundTarget` | string | `"section"` | `"section"` paints background/border/shadow on the section; `"content"` paints them on the inner content box (`.lcms-section__grid`) — Divi-style "row card" |
| `contentPaddingTop/Right/Bottom/Left` | number | `0` | Inner padding of the content box (independent of the section padding) |
| `contentPaddingUnit` | string | `"px"` | Unit for the content padding: `"px"`, `"%"`, `"vw"`, `"em"` |
| `columnGapUnit` | string | `"px"` | Unit of `columnGap`: `"px"` or `"%"` |
| `paddingUnit` | string | `"px"` | Unit shared by the four padding values: `"px"`, `"%"`, `"vw"`, `"em"` |
| `marginUnit` | string | `"px"` | Unit shared by the four margin values: `"px"`, `"%"`, `"vw"`, `"em"` |
| `columnGap` | number | `0` | Gap between columns in pixels |
| `stackOnTablet` | boolean | `false` | Stack columns vertically on tablet |
| `stackOnMobile` | boolean | `true` | Stack columns vertically on mobile |
| `sticky` | boolean | `false` | Section sticks to top on scroll |
| `stickyTop` | number | `0` | Distance from top when sticky (pixels) |
| `stickyZIndex` | number | `100` | Z-index when sticky |
| `scrolledBg` | string | `""` | Background color after scrolling (only when sticky) |
| `scrolledShadow` | string | `"none"` | Box shadow after scrolling: `"none"`, `"sm"`, `"md"`, `"lg"` (only when sticky) |
| `cssId` | string | `""` | HTML ID attribute for anchor links (e.g., "contact" allows linking via #contact) |
| `cssClass` | string | `""` | Custom CSS class(es) for styling |

### Column Settings

Columns are inside sections. Each column has:

```json
{
  "uuid": "column-uuid",
  "span": 12,
  "settings": {
    // Size
    "columnHeight": null,
    "minHeight": null,

    // Background (same properties as section)
    "backgroundColor": "",
    "backgroundOpacity": 100,
    "backgroundImage": "",
    "backgroundSize": "cover", // "cover" | "contain" | "auto" | raw CSS like "200px auto" or "30% auto"
    "backgroundPosition": "center center",
    "backgroundImageOpacity": 100,
    "useGradient": false,
    "gradientType": "linear",
    "gradientAngle": 180,
    "gradientColorStart": "#667eea",
    "gradientColorEnd": "#764ba2",

    // Spacing (in pixels)
    "paddingTop": 0,
    "paddingRight": 0,
    "paddingBottom": 0,
    "paddingLeft": 0,
    "marginTop": 0,
    "marginRight": 0,
    "marginBottom": 0,
    "marginLeft": 0,

    // Alignment
    "verticalAlign": "flex-start",
    "horizontalAlign": "stretch",

    // Border (same as section)
    "borderRadius": 0,
    "borderWidth": 0,
    "borderColor": "#000000",
    "borderStyle": "solid",
    "boxShadow": "",

    // Responsive
    "hidden": false,
    "responsive": {
      "tablet": { "hidden": false },
      "mobile": { "hidden": false }
    },

    // Link (clickable column)
    "link": {
      "enabled": false,
      "type": "custom",
      "url": "",
      "page_id": null,
      "collection_code": null,
      "entry_id": null,
      "route_uuid": null,
      "target_blank": false
    },

    // Advanced
    "cssId": "",
    "cssClass": ""
  },
  "content": [...]
}
```

#### Column Settings Reference

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `span` | number | `12` | Column span in a 12-column grid (e.g., 6 = 50% width) |
| `columnHeight` | number/null | `null` | Fixed column height in pixels |
| `minHeight` | number/null | `null` | Minimum column height in pixels |
| `maxWidth` | number/null | `null` | Max width of the column box inside its grid track (API emits it with the unit, e.g. `"62%"`) |
| `widgetGap` | number/null | `null` | Vertical gap between the column's widgets in px; `0` is allowed, unset = theme default (1rem) |
| `maxWidthUnit` | string | `"px"` | Unit of `maxWidth`: `"px"`, `"%"`, `"vw"` |
| `paddingUnit` | string | `"px"` | Unit shared by the four padding values: `"px"`, `"%"`, `"vw"`, `"em"` |
| `marginUnit` | string | `"px"` | Unit shared by the four margin values: `"px"`, `"%"`, `"vw"`, `"em"` |
| `verticalAlign` | string | `"flex-start"` | Vertical alignment ("flex-start", "center", "flex-end") |
| `horizontalAlign` | string | `"stretch"` | Horizontal alignment ("flex-start", "center", "flex-end", "stretch") |
| `cssId` | string | `""` | HTML ID attribute for anchor links |
| `cssClass` | string | `""` | Custom CSS class(es) for styling |
| `sticky` | boolean | `false` | Enable sticky positioning for the column |
| `stickyTop` | number | `0` | Top offset in pixels when sticky (e.g., 100 for sticky navbar clearance) |
| `stickyZIndex` | number | `10` | Z-index of the sticky column |

### Widget Settings

Widgets are inside columns. Each widget has:

```json
{
  "uuid": "widget-uuid",
  "widget_type": "heading",
  "widget": {
    "heading": {
      "html": { "pl": "<h2>Nagłówek</h2>", "en": "<h2>Heading</h2>" },
      "color": "var:dark",
      "color:hover": null
    },
    "config": {
      "content_source": "static",
      "level": 2
    }
  },
  "settings": {
    // Size
    "height": 0,
    "minHeight": 0,
    "autoHeight": true,
    "fullHeight": false,

    // Background (same properties as section/column)
    "backgroundColor": "",
    "backgroundOpacity": 100,
    "backgroundImage": "",
    "backgroundSize": "cover", // "cover" | "contain" | "auto" | raw CSS like "200px auto" or "30% auto"
    "backgroundPosition": "center center",
    "backgroundImageOpacity": 100,
    "useGradient": false,
    "gradientType": "linear",
    "gradientAngle": 180,
    "gradientColorStart": "#667eea",
    "gradientColorEnd": "#764ba2",

    // Spacing (in pixels)
    "paddingTop": 0,
    "paddingRight": 0,
    "paddingBottom": 0,
    "paddingLeft": 0,
    "marginTop": 0,
    "marginRight": 0,
    "marginBottom": 0,
    "marginLeft": 0,

    // Alignment
    "verticalAlign": "top",
    "horizontalAlign": "stretch",

    // Border (same as section/column)
    "borderRadius": 0,
    "borderWidth": 0,
    "borderColor": "#000000",
    "borderStyle": "solid",
    "boxShadow": "",

    // Hover effects
    "hover": {
      "backgroundColor": "",
      "backgroundOpacity": 100,
      "borderColor": "",
      "borderWidth": null,
      "boxShadow": "",
      "transitionDuration": 300
    },

    // Responsive
    "hidden": false,
    "responsive": {
      "tablet": { "hidden": false, "paddingTop": 10 },
      "mobile": { "hidden": false, "paddingTop": 5 }
    },

    // Link (clickable widget)
    "link": {
      "enabled": false,
      "type": "custom",
      "url": "",
      "page_id": null,
      "collection_code": null,
      "entry_id": null,
      "route_uuid": null,
      "target_blank": false
    },

    // Advanced
    "cssId": "",
    "cssClass": ""
  }
}
```

#### Widget Settings Reference

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `height` | number | `0` | Fixed height in pixels (0 = auto) |
| `minHeight` | number | `0` | Minimum height in pixels |
| `autoHeight` | boolean | `true` | Height adjusts to content |
| `fullHeight` | boolean | `false` | Widget takes full parent height |
| `verticalAlign` | string | `"top"` | Vertical alignment ("top", "center", "bottom") |
| `horizontalAlign` | string | `"stretch"` | Horizontal alignment ("flex-start", "center", "flex-end", "stretch") |
| `cssId` | string | `""` | HTML ID attribute for anchor links |
| `cssClass` | string | `""` | Custom CSS class(es) for styling |

### Responsive Settings

All three levels (section, column, widget) support responsive overrides. Settings in `responsive.tablet` or `responsive.mobile` override base settings for those breakpoints:

```json
{
  "settings": {
    "paddingTop": 40,
    "hidden": false,
    "responsive": {
      "tablet": {
        "paddingTop": 20,
        "hidden": false
      },
      "mobile": {
        "paddingTop": 10,
        "hidden": true
      }
    }
  }
}
```

Breakpoints:
- **Desktop**: > 1024px
- **Tablet**: 768px - 1024px
- **Mobile**: < 768px

### Hover Settings

Widgets support hover effects that are applied when the user hovers over the element. Hover settings are stored in the `settings.hover` object:

```json
{
  "settings": {
    "hover": {
      "backgroundColor": "#f0f0f0",
      "backgroundOpacity": 100,
      "borderColor": "#007bff",
      "borderWidth": 2,
      "boxShadow": "0 4px 6px rgba(0,0,0,0.1)",
      "transitionDuration": 300
    },
    "responsive": {
      "tablet": {
        "hover": {
          "backgroundColor": "#e0e0e0"
        }
      },
      "mobile": {
        "hover": {
          "boxShadow": ""
        }
      }
    }
  }
}
```

#### Hover Settings Reference

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `backgroundColor` | string | `""` | Background color on hover |
| `backgroundOpacity` | number | `100` | Background opacity on hover (0-100) |
| `borderColor` | string | `""` | Border color on hover |
| `borderWidth` | number/null | `null` | Border width on hover in pixels (null = no change) |
| `boxShadow` | string | `""` | Box shadow on hover (CSS value) |
| `transitionDuration` | number | `300` | Transition animation duration in milliseconds |

Widget-level `settings` may additionally carry `lift:hover` (px), `scale:hover` (factor), `shadow_preset:hover` (`none|sm|md|lg`) and `animation:hover` — a looping animation while hovered: `pulse` (`scale 1 → 1.1 → 0.9`, 1s linear infinite), `bob` (translateY −8px ↔ −4px, 1.5s infinite) or `wobble` (small rotate, 1s once). Render `animation:hover` as `element:hover { animation: <keyframes> }`.

#### Rendering Hover Effects

To render hover effects, generate a dynamic `<style>` tag with CSS `:hover` rules:

```javascript
function generateHoverCss(widgetId, hover) {
  if (!hover || (!hover.backgroundColor && !hover.borderColor && !hover.boxShadow)) {
    return ''
  }

  const transitionDuration = hover.transitionDuration ?? 300

  let css = `#${widgetId} { transition: all ${transitionDuration}ms ease; }`
  css += `#${widgetId}:hover {`

  if (hover.backgroundColor) {
    const opacity = hover.backgroundOpacity ?? 100
    if (opacity < 100) {
      css += `background-color: ${hexToRgba(hover.backgroundColor, opacity / 100)};`
    } else {
      css += `background-color: ${hover.backgroundColor};`
    }
  }

  if (hover.borderColor) {
    css += `border-color: ${hover.borderColor};`
  }

  if (hover.borderWidth !== undefined && hover.borderWidth !== null) {
    css += `border-width: ${hover.borderWidth}px;`
  }

  if (hover.boxShadow) {
    css += `box-shadow: ${hover.boxShadow};`
  }

  css += '}'
  return css
}
```

### Content-Level Hover Effects

Card-type widgets (service-card, cta-box, pill, icon-box, numbered-box, hero, counter, progress-bar, tabs, timeline, blockquote, pricing-table, icon-list) support content-level hover effects stored in the `widget` object:

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `hover_background_color` | string/null | `null` | Background color on hover |
| `hover_text_color` | string/null | `null` | Text color on hover |
| `hover_lift` | number | `0` | Lift effect in pixels (translateY offset) |
| `hover_scale` | number | `1` | Scale factor on hover (e.g. `1.05`) |
| `hover_shadow` | string | `"none"` | Shadow preset: `"none"`, `"sm"`, `"md"`, `"lg"` |
| `transition_duration` | number | `200` | Transition duration in milliseconds |

Shadow preset values:
- `sm`: `0 4px 6px -1px rgba(0,0,0,0.1), 0 2px 4px -1px rgba(0,0,0,0.06)`
- `md`: `0 10px 15px -3px rgba(0,0,0,0.1), 0 4px 6px -2px rgba(0,0,0,0.05)`
- `lg`: `0 20px 25px -5px rgba(0,0,0,0.1), 0 10px 10px -5px rgba(0,0,0,0.04)`

```javascript
// Render content-level hover via CSS custom properties
function applyContentHover(element, widget) {
  const d = widget.widget
  const dur = `${d.transition_duration ?? 200}ms`

  element.style.transition = `background-color ${dur} ease, color ${dur} ease, transform ${dur} ease, box-shadow ${dur} ease`

  if (d.hover_background_color) element.style.setProperty('--hover-bg', d.hover_background_color)
  if (d.hover_text_color) element.style.setProperty('--hover-color', d.hover_text_color)
  if (d.hover_lift) element.style.setProperty('--hover-lift', `-${d.hover_lift}px`)
  if (d.hover_scale && d.hover_scale !== 1) element.style.setProperty('--hover-scale', d.hover_scale)

  const shadowMap = {
    sm: '0 4px 6px -1px rgba(0,0,0,0.1), 0 2px 4px -1px rgba(0,0,0,0.06)',
    md: '0 10px 15px -3px rgba(0,0,0,0.1), 0 4px 6px -2px rgba(0,0,0,0.05)',
    lg: '0 20px 25px -5px rgba(0,0,0,0.1), 0 10px 10px -5px rgba(0,0,0,0.04)'
  }
  if (d.hover_shadow && shadowMap[d.hover_shadow]) {
    element.style.setProperty('--hover-shadow', shadowMap[d.hover_shadow])
  }

  // Add CSS rule:
  // .widget:hover {
  //   background-color: var(--hover-bg);
  //   color: var(--hover-color);
  //   transform: translateY(var(--hover-lift, 0)) scale(var(--hover-scale, 1));
  //   box-shadow: var(--hover-shadow, none);
  // }
}
```

**Note:** Content-level hover (widget colors/transforms) is separate from wrapper-level hover (settings.hover). Both can be active simultaneously.

### Link Settings

Sections, columns, and widgets can be made clickable:

| Link Type | Properties Used |
|-----------|-----------------|
| `custom` | `url` - external or relative URL |
| `page` | `page_id` - UUID of internal page |
| `collection` | `collection_code` + `entry_id` - link to collection entry |
| `route` | `route_uuid` - UUID of defined route |

## Widget Types to Support

All widgets follow this structure:
```json
{
  "uuid": "unique-id",
  "widget_type": "heading",
  "widget": {
    "heading": {
      "html": { "pl": "<h2>Nagłówek</h2>", "en": "<h2>Heading</h2>" },
      "color": "var:dark",
      "color:hover": null
    },
    "config": {
      "content_source": "static",
      "level": 2
    }
  },
  "settings": {
    "paddingTop": 20,
    "marginBottom": 10,
    "backgroundColor": "#fff",
    ...
  }
}
```

The `widget` key contains widget-specific data organized in element-groups (e.g. `heading`, `text`, `icon`) with color/hover support, plus a `config` group for non-visual settings. The `settings` key contains container styling (padding, margin, background, border, etc.).

### Multi-Item Widgets

Some widgets (counter, button, icon-box, numbered-box, progress-bar, service-card, team-member, pricing-table, pill, link) support a multi-item mode where multiple instances are displayed in a CSS Grid. Check for `multi_item: true`:

```json
{
  "uuid": "unique-id",
  "widget_type": "counter",
  "multi_item": true,
  "multi_columns": 3,
  "multi_gap": 16,
  "items": [
    { "widget_type": "counter", "widget": {...} },
    { "widget_type": "counter", "widget": {...} },
    { "widget_type": "counter", "widget": {...} }
  ],
  "settings": {...}
}
```

When rendering, wrap items in a grid:
```html
<div style="display: grid; grid-template-columns: repeat(3, 1fr); gap: 16px;">
  <!-- render each item as a normal widget -->
</div>
```

### Basic Widgets
- `heading` - h1-h6 with text (multilingual)
- `text` - Rich HTML content (multilingual)
- `button` - Link button with style/size
- `image` - Image with alt text and style presets (rounded, shadow, border, etc.)
- `icon` - Font Awesome icon
- `divider` - Horizontal line
- `spacer` - Vertical space
- `icon-box` - Icon + content block
- `numbered-box` - Auto-numbered (01, 02...) + content block
- `link` - Clickable link with optional icon and hover animations
- `service-card` - Card with icon, title, description, link and badge
- `pill` - Inline badge/pill with text and background
- `cta-box` - Call-to-action box with heading and link
- `feature-list` - Bulleted list of features/benefits
- `icon-list` - List of items with icons and text
- `blockquote` - Quote/testimonial text with optional attribution
- `timeline` - Vertical timeline with events and dates
- `family-tree` - Hierarchical tree structure with connected nodes
- `table` - Data table with rows/columns and optional header/footer
- `embed` - Embedded HTML/iframe content
- `team-member` - Card displaying person info (photo, name, role, bio)
- `breadcrumbs` - Navigation breadcrumb trail
- `toc` - Table of contents with anchor links
- `block-content` - Reusable content block (fetches from /blocks/{code})
- `pricing-table` - Pricing plans table with features and CTA buttons
- `animated-headline` - Premium heading with a rotating / typed / highlighted word (plan feature `premium_widgets`)
- `flip-box` - Premium two-sided card revealed by flip / slide / zoom (plan feature `premium_widgets`)

### Media Widgets
- `video` - YouTube/Vimeo/URL embed
- `audio` - Audio player with optional playlist (streamed with Range support)
- `gallery` - Image grid
- `google-maps` - Embedded map
- `google-reviews` - Google Reviews (grid/carousel/list layout with rating summary)
- `pdf-viewer` - Embedded PDF viewer with zoom/download controls
- `openstreetmap` - OpenStreetMap with customizable markers and GeoJSON overlay
- `before-after` - Premium before/after image compare with draggable divider (plan feature `premium_widgets`)
- `hotspots` - Premium image with pinned markers and tooltips (plan feature `premium_widgets`)
- `video-lightbox` - Premium poster + play button opening the video in a lightbox (plan feature `premium_widgets`)
- `lottie` - Premium Lottie JSON animation with viewport / hover / click / scroll triggers (plan feature `premium_widgets`)

### Layout Widgets
- `hero` - Full-width hero section
- `hero-slider` - Premium multi-slide hero with layered entrance animations (plan feature `premium_widgets`)
- `grid` - Nested grid with columns containing widgets
- `wrapper` - Flat container of widgets (`data.layout` `grid`|`inline`, `data.columns`, `gap`, `equal_height`, `columns_tablet/mobile`); wrappers cannot be nested. `style.background_image` (+ `background_size`, `background_position`, `background_image_opacity`) and `style.gradient` paint the wrapper box like a column — with opacity < 100 the image goes to a `::before` layer (`has-bg-image-opacity`), children get `z-index: 1`. `equal_height` stretches every cell (`grid-auto-rows: 1fr`, first child `flex: 1`).

### Collection Widgets
- `collection-grid` - Grid of collection entries. Its responsive class is deterministic (`lcms-grid-c{cols}-t{tablet}-m{mobile}`), so SSR and client markup match; option/relation values are enriched objects (`{code,label}` / `{entry_id,…}`), the card shows the label.
- `collection-carousel` - Carousel of entries
- `collection-single` - Single entry display
- `collection-field` - Single field from entry (fetches from collection entry)
- `collection-grouped` - Entries grouped by field
- `value-list` - Unique values from field

### Navigation
- `menu` - Navigation menu (fetches from /menus/{code})
- `mega-menu` - Premium navigation bar with per-item simple/mega dropdown panels, featured cards and a mobile drawer; items from /menus/{code} (plan feature `premium_widgets`)
- `social-icons` - Social media links
- `off-canvas` - Premium trigger + side drawer with menu and rich text (plan feature `premium_widgets`)
- `language-switcher` - Links to the current page in other languages (dropdown or inline). Build the list from the entry's `metadata.urls` or the page's route `url_translation` (with `/{lang}` prefix for non-default languages); show only languages that have an address, render nothing when there is only one

### Multilingual links
- Language = first URL segment (`/en/...`); no prefix = project `default_language`
- Pass `?lang={current}` to `/pages/{code}`, `/menus/{code}` and `/collections/{code}` so links in widgets, menus and entry lists point to the current language
- Entry URL in a language: `metadata.urls[lang]` if present, otherwise `metadata.url` (untranslated entries link to the default-language version)

### Interactive
- `countdown` - Countdown timer
- `counter` - Animated number counter
- `progress-bar` - Progress indicator
- `testimonial` - Quote with author
- `alert` - Notification box
- `form` - Contact form (fetches definition from /forms/{code}), supports full input styling (background, text, border, focus, placeholder colors)
- `accordion` - Expandable/collapsible content sections
- `tabs` - Tabbed content interface

### Content Widgets
- `business-info` - Business contact information (address, phone, email, hours)
- `data-field` - Display single data field with label
- `cookie-consent` - Cookie consent banner with customizable message and settings

### E-Commerce Widgets
**Note:** Requires active LessCommerce account linked to project.

#### Product Widgets
- `product-grid` - Grid of products (latest, category, featured, manual, or search results)
- `product-carousel` - Carousel/slider of products
- `product-detail` - Single product with gallery, variants, price, add-to-cart
- `product-variants` - Product variant selector with stock status
- `product-configurator` - Premium product configuration tool with options and pricing (plan feature `premium_widgets`)
- `product-gallery` - Product image gallery with lightbox
- `product-price` - Single product price display
- `product-cart` - Add-to-cart button with quantity selector
- `product-field` - Display single product field

#### Category & Search Widgets
- `category-grid` - Grid of product categories
- `category-header` - Category title and description
- `category-tree` - Hierarchical category navigation
- `related-products` - Products related to current product
- `search-bar` - Product search input field
- `search-icon` - Search icon trigger/button

#### Cart & Checkout Widgets
- `mini-cart` - Shopping cart preview/dropdown
- `cart` - Full shopping cart page with items and summary
- `checkout` - Checkout form (payment, shipping, billing)
- `order-success` - Order confirmation message
- `order-failure` - Order error/cancellation message
- `customer-account` - Customer profile and order history

#### Misc E-Commerce Widgets
- `account-icon` - User account menu/button
- `ecommerce-icons` - Product icons/badges (new, sale, etc.)

#### E-Commerce Forms
- `login-form` - Customer login form
- `register-form` - Customer registration form

### Navigation & Marketing Widgets
- `nav-bar` - Navigation bar (differs from `menu` — direct item config vs /menus/{code} fetch)
- `marketing-topbar` - Marketing announcement banner
- `campaign-banner` - Campaign-specific banner with CTA

## CSS Structure

```css
/* lcms-styles.css */

/* Variables from /config */
:root {
  --lcms-primary-color: #007BFF;
  --lcms-secondary-color: #6c757d;
  --lcms-font-heading: 'Playfair Display', serif;
  --lcms-font-body: 'Inter', sans-serif;
  --lcms-border-radius: 8px;
}

/* Sections */
.lcms-section { ... }
.lcms-section-inner { ... }
.lcms-column { ... }

/* Widgets */
.lcms-widget { ... }
.lcms-widget-heading { ... }
.lcms-widget-heading h1 { ... }
.lcms-widget-heading h2 { ... }
.lcms-widget-text { ... }
.lcms-widget-button { ... }
.lcms-widget-button-primary { ... }
.lcms-widget-image { ... }
.lcms-widget-hero { ... }
.lcms-widget-collection-grid { ... }
/* etc for all widgets */

/* Responsive */
@media (max-width: 768px) {
  .lcms-column { width: 100% !important; }
}
```

## Data Flow

1. App loads → fetch /config → set CSS variables
2. App loads → fetch /routes → store in state
3. URL changes → match route → determine type (page/collection)
4. If page → fetch /pages/{target} → render sections
5. If collection list → fetch /collections/{target}/entries → render grid
6. If collection detail → extract slug → fetch entry → render with template

## Language Handling

- Get language from URL prefix (/en/, /pl/) or cookie/localStorage
- Pass language to all fetch calls
- Widget data is in `widget.widget` — multilingual fields are objects with language keys
- Access: `widget.widget.html[language]`
- Fallback: `widget.widget.html[language] || widget.widget.html.en || ''`
- Non-multilingual fields are simple values: `widget.widget.level`, `widget.widget.style`, etc.

## File Structure

```
/src
  /api
    client.js          # API client with auth header
  /components
    WidgetRenderer.js  # Main widget switch
    SectionRenderer.js # Renders sections with columns
    /widgets
      Heading.js
      Text.js
      Button.js
      Image.js
      Hero.js
      CollectionGrid.js
      ... (one per widget type)
  /styles
    lcms-styles.css    # ALL styles in one file with lcms- prefix
  /utils
    router.js          # Route matching logic
  App.js               # Main app with routing
  index.js
```

## Generate the complete application following these specifications.
````

## Why This Approach?

### 1. Universal Engine
One frontend works for any LessCMS project - just change the API key.

### 2. Route-Driven
No hardcoded pages. Routes from API define the entire site structure.

### 3. Style Integration
The `lcms-` prefixed CSS can be loaded into the LessCMS page builder to show real styles while editing. When user customizes styles in LessCMS, the CSS file is regenerated.

### 4. Widget Modularity
Each widget is a separate component. Adding new widget types is straightforward.

## CSS Integration with Page Builder

The generated `lcms-styles.css` file serves two purposes:

1. **Frontend website** - Loaded normally as site styles
2. **LessCMS Page Builder** - Injected into the builder iframe to preview real styles

```
┌─────────────────────────────────────────────┐
│ LessCMS Page Builder                        │
│ ┌─────────────────────────────────────────┐ │
│ │ <iframe>                                │ │
│ │   <link href="lcms-styles.css">        │ │
│ │   <div class="lcms-widget-heading">    │ │
│ │     <h1>Preview with real styles</h1>  │ │
│ │   </div>                                │ │
│ │ </iframe>                               │ │
│ └─────────────────────────────────────────┘ │
└─────────────────────────────────────────────┘
```

## After Generation

1. **Test route matching** - Verify all URL patterns work
2. **Check widget rendering** - Test each widget type
3. **Verify CSS prefix** - Ensure all classes start with `lcms-`
4. **Test language switching** - Check multilingual content
5. **Validate config styles** - Confirm CSS variables from /config work
6. **Test in page builder** - Load CSS in LessCMS preview

## Example Generated Widget

```jsx
// widgets/Heading.js
export function Heading({ content, config, settings, language }) {
  // Content is multilingual (html field with language keys)
  const html = content.html?.[language] || content.html?.en || '';
  // Config contains non-multilingual settings
  const level = config.level || 2;
  const Tag = `h${level}`;

  const style = {
    textAlign: settings.textAlign,
    marginTop: settings.marginTop,
    marginBottom: settings.marginBottom,
  };

  // If content is HTML, render with dangerouslySetInnerHTML
  const isHtml = html.includes('<');

  return (
    <div className="lcms-widget lcms-widget-heading" style={style}>
      {isHtml ? (
        <Tag
          className={`lcms-heading lcms-heading-${level}`}
          dangerouslySetInnerHTML={{ __html: html }}
        />
      ) : (
        <Tag className={`lcms-heading lcms-heading-${level}`}>
          {html}
        </Tag>
      )}
    </div>
  );
}
```

```css
/* In lcms-styles.css */
.lcms-widget-heading {
  width: 100%;
}

.lcms-heading {
  font-family: var(--lcms-font-heading);
  color: var(--lcms-text-color);
  line-height: 1.3;
}

.lcms-heading-1 { font-size: 48px; }
.lcms-heading-2 { font-size: 36px; }
.lcms-heading-3 { font-size: 28px; }
.lcms-heading-4 { font-size: 24px; }
.lcms-heading-5 { font-size: 20px; }
.lcms-heading-6 { font-size: 18px; }
```

## Resources

- [Routes API](reference/routes.md) - Route definitions
- [Config API](reference/config.md) - Project configuration
- [Pages API](reference/pages.md) - Page structure
- [Widgets Reference](reference/widgets/) - All widget types
