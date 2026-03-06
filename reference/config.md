# Config

Project configuration and settings for frontend rendering.

## Endpoint

<span class="endpoint-badge get">GET</span> `/v1/:workspace_code/:project_code/config`

## Get Project Config

Returns project configuration including fonts, widgets, routing schemas, and homepage settings.

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace/project/config
```

### Response

```json
{
  "data": {
    "styles": {
      "primary_color": "#007BFF",
      "secondary_color": "#6c757d",
      "accent_color": "#FF6B35",
      "success_color": "#28a745",
      "danger_color": "#dc3545",
      "warning_color": "#ffc107",
      "info_color": "#50a5f1",
      "light_color": "#f8f9fa",
      "dark_color": "#343a40",
      "white_color": "#ffffff",
      "black_color": "#000000",
      "text_color": "#333333",
      "background_color": "#ffffff",
      "background_alt_color": "#f8f9fa",
      "link_color": "#007BFF",
      "muted_color": "#6c757d",
      "border_color": "#dee2e6",
      "custom_colors": [
        { "name": "Brand Gold", "color": "#c9a227" }
      ],
      "font_heading": "Playfair Display",
      "font_body": "Inter",
      "font_size_base": 16,
      "line_height": 1.6,
      "h1_font_size": "clamp(2rem, 4vw, 3rem)",
      "h1_font_weight": 700,
      "h1_color": "#212529",
      "h2_font_size": "1.75rem",
      "h2_font_weight": 600,
      "h2_color": null,
      "p_font_size": "1rem",
      "p_font_weight": 400,
      "p_color": null,
      "p_line_height": "1.6",
      "border_radius": 8,
      "container_max_width": 1200,
      "section_gap": "40px",
      "btn_padding": "10px 24px",
      "btn_border_radius": "6px",
      "btn_font_size": "14px",
      "btn_font_weight": 500,
      "link_hover_color": "#0056b3",
      "link_text_decoration": "none",
      "link_hover_text_decoration": "underline"
    },
    "fonts": ["Inter", "Playfair Display"],
    "custom_css_urls": ["https://cdn.example.com/custom.css"],
    "custom_css": ".my-class { color: red; font-weight: bold; }",
    "available_widgets": [
      "text", "heading", "image", "gallery", "button", "link", "pill",
      "divider", "spacer", "blockquote", "icon-list", "video",
      "hero", "grid", "block-content",
      "countdown", "counter", "progress-bar", "testimonial",
      "alert", "accordion", "tabs", "timeline", "table", "embed", "form",
      "service-card", "icon-box", "numbered-box", "cta-box", "feature-list", "team-member",
      "pricing-table", "menu", "social-icons", "breadcrumbs", "toc",
      "google-maps", "openstreetmap", "pdf-viewer",
      "collection-grid", "collection-carousel", "collection-single",
      "collection-grouped", "value-list", "data-field"
    ],
    "available_fonts": [
      "Inter", "Roboto", "Open Sans", "Lato", "Montserrat", "Poppins",
      "Raleway", "Oswald", "Source Sans Pro", "Nunito", "Ubuntu",
      "Merriweather", "Playfair Display", "PT Sans", "Lora", "Noto Sans",
      "Fira Sans", "Work Sans", "Rubik", "Quicksand", "Mulish", "Barlow",
      "Karla", "Inconsolata", "Space Grotesk", "DM Sans", "Manrope",
      "Outfit", "Plus Jakarta Sans", "Lexend"
    ],
    "google_fonts_url": "https://fonts.googleapis.com/css2?family=Inter:wght@100;200;300;400;500;600;700;800;900&family=Playfair+Display:wght@100;200;300;400;500;600;700;800;900&display=swap",
    "page_route_schema": "/p/{slug}",
    "collection_route_schema": "/c/{collection_code}/{slug}",
    "homepage_uuid": "550e8400-e29b-41d4-a716-446655440000",
    "color_variables": [
      { "code": "primary", "label": "Primary", "value": "#007BFF" },
      { "code": "secondary", "label": "Secondary", "value": "#6c757d" },
      { "code": "success", "label": "Success", "value": "#28a745" },
      { "code": "danger", "label": "Danger", "value": "#dc3545" },
      { "code": "warning", "label": "Warning", "value": "#ffc107" },
      { "code": "info", "label": "Info", "value": "#50a5f1" },
      { "code": "light", "label": "Light", "value": "#f8f9fa" },
      { "code": "dark", "label": "Dark", "value": "#343a40" }
    ],
    "languages": ["pl", "en"],
    "default_language": "pl"
  }
}
```

> **Note:** The `available_widgets` list may vary per project if the project has custom widget settings. The list above shows the default set. The `styles` object only contains fields that have been configured — unconfigured fields will be absent (not `null`).

### Response Fields

| Field | Type | Description |
|-------|------|-------------|
| `styles` | object | Theme style settings (colors, typography, layout) — see [Styles Object Fields](#styles-object-fields) |
| `fonts` | array | Google Font family names enabled for this project |
| `custom_css_urls` | array | URLs to external custom stylesheets |
| `custom_css` | string\|null | Inline custom CSS code for the project (`null` if not set) |
| `available_widgets` | array | Widget type codes enabled for page builder |
| `available_fonts` | array | All available Google Fonts for font picker |
| `google_fonts_url` | string | Pre-built Google Fonts URL for `<link>` tag |
| `page_route_schema` | string | URL pattern for pages (e.g., `/p/{slug}`) |
| `collection_route_schema` | string | URL pattern for collection entries (e.g., `/c/{collection_code}/{slug}`) |
| `homepage_uuid` | string\|null | UUID of the page set as homepage (`null` if not set) |
| `color_variables` | array | Color variables for theming — see [Color Variables](#color-variables) |
| `languages` | array | Languages enabled for this project (e.g., `["pl", "en"]`) |
| `default_language` | string | Default language code (e.g., `"pl"`) |

### Response Headers

```
Cache-Tag: project-{project_uuid} project-{project_uuid}-config
```

---

## Use Cases

### Load Google Fonts

```html
<!-- Add to <head> -->
<link rel="stylesheet" href="${config.google_fonts_url}">
```

### Initialize Page Builder

```javascript
const config = await fetchConfig();

// Filter widgets in page builder
const availableWidgets = config.data.available_widgets;

// Setup font picker
const fontOptions = config.data.available_fonts;
```

### Build Page URLs

```javascript
function buildPageUrl(slug) {
  const pattern = config.data.page_route_schema; // "/p/{slug}"
  return pattern.replace('{slug}', slug);
}

function buildCollectionEntryUrl(collectionCode, slug) {
  const pattern = config.data.collection_route_schema; // "/c/{collection_code}/{slug}"
  return pattern
    .replace('{collection_code}', collectionCode)
    .replace('{slug}', slug);
}
```

### Resolve Color Variables

Widget configs may reference colors as `"var:primary"`, `"var:accent"`, etc. Use `color_variables` to resolve them:

```javascript
function resolveColor(colorValue, colorVariables) {
  if (!colorValue) return null;
  if (colorValue.startsWith('var:')) {
    const code = colorValue.slice(4); // "var:primary" → "primary"
    const cv = colorVariables.find(c => c.code === code);
    return cv ? cv.value : colorValue;
  }
  return colorValue; // already a hex value
}

// Usage
const config = await fetchConfig();
const bgColor = resolveColor(widget.config.background_color, config.data.color_variables);
// "var:primary" → "#007BFF"
// "#ff0000" → "#ff0000"
```

### Detect Homepage

```javascript
async function isHomepage(pageUuid) {
  const config = await fetchConfig();
  return config.data.homepage_uuid === pageUuid;
}
```

### Load Custom CSS

```javascript
const config = await fetchConfig();

// Load external CSS stylesheets
config.data.custom_css_urls.forEach(url => {
  const link = document.createElement('link');
  link.rel = 'stylesheet';
  link.href = url;
  document.head.appendChild(link);
});

// Load inline custom CSS
if (config.data.custom_css) {
  const style = document.createElement('style');
  style.dataset.lesscmsCustomCss = 'true';
  style.textContent = config.data.custom_css;
  document.head.appendChild(style);
}
```

### Apply Theme Styles (CSS Variables)

```javascript
const config = await fetchConfig();
const { styles, color_variables } = config.data;
const root = document.documentElement;

// Apply color variables (var:primary, var:secondary, etc.)
color_variables.forEach(cv => {
  root.style.setProperty(`--lcms-${cv.code}`, cv.value);
});

// Apply all color styles
const colorFields = [
  'primary_color', 'secondary_color', 'accent_color',
  'success_color', 'danger_color', 'warning_color', 'info_color',
  'light_color', 'dark_color', 'white_color', 'black_color',
  'text_color', 'background_color', 'background_alt_color',
  'link_color', 'muted_color', 'border_color'
];
colorFields.forEach(field => {
  if (styles[field]) {
    root.style.setProperty(`--lcms-${field.replace(/_/g, '-')}`, styles[field]);
  }
});

// Typography
if (styles.font_heading) root.style.setProperty('--lcms-font-heading', `'${styles.font_heading}', sans-serif`);
if (styles.font_body) root.style.setProperty('--lcms-font-body', `'${styles.font_body}', sans-serif`);
if (styles.font_size_base) root.style.setProperty('--lcms-font-size-base', `${styles.font_size_base}px`);
if (styles.line_height) root.style.setProperty('--lcms-line-height', styles.line_height);

// Heading typography (H1-H6)
for (let i = 1; i <= 6; i++) {
  if (styles[`h${i}_font_size`]) root.style.setProperty(`--lcms-h${i}-font-size`, styles[`h${i}_font_size`]);
  if (styles[`h${i}_font_weight`]) root.style.setProperty(`--lcms-h${i}-font-weight`, styles[`h${i}_font_weight`]);
  if (styles[`h${i}_color`]) root.style.setProperty(`--lcms-h${i}-color`, styles[`h${i}_color`]);
}

// Paragraph typography
if (styles.p_font_size) root.style.setProperty('--lcms-p-font-size', styles.p_font_size);
if (styles.p_font_weight) root.style.setProperty('--lcms-p-font-weight', styles.p_font_weight);
if (styles.p_color) root.style.setProperty('--lcms-p-color', styles.p_color);
if (styles.p_line_height) root.style.setProperty('--lcms-p-line-height', styles.p_line_height);

// Layout
if (styles.border_radius !== undefined) root.style.setProperty('--lcms-border-radius', `${styles.border_radius}px`);
if (styles.container_max_width) root.style.setProperty('--lcms-container-max-width', `${styles.container_max_width}px`);
if (styles.section_gap) root.style.setProperty('--lcms-section-gap', styles.section_gap);

// Button defaults
if (styles.btn_padding) root.style.setProperty('--lcms-btn-padding', styles.btn_padding);
if (styles.btn_border_radius) root.style.setProperty('--lcms-btn-border-radius', styles.btn_border_radius);
if (styles.btn_font_size) root.style.setProperty('--lcms-btn-font-size', styles.btn_font_size);
if (styles.btn_font_weight) root.style.setProperty('--lcms-btn-font-weight', styles.btn_font_weight);

// Link defaults
if (styles.link_hover_color) root.style.setProperty('--lcms-link-hover-color', styles.link_hover_color);
if (styles.link_text_decoration) root.style.setProperty('--lcms-link-text-decoration', styles.link_text_decoration);
if (styles.link_hover_text_decoration) root.style.setProperty('--lcms-link-hover-text-decoration', styles.link_hover_text_decoration);
```

### Example CSS Using Variables

```css
:root {
  /* Fallback values — overridden by JavaScript from /config */

  /* Brand colors */
  --lcms-primary-color: #007BFF;
  --lcms-secondary-color: #6c757d;
  --lcms-accent-color: #FF6B35;

  /* Semantic colors */
  --lcms-success-color: #28a745;
  --lcms-danger-color: #dc3545;
  --lcms-warning-color: #ffc107;
  --lcms-info-color: #50a5f1;

  /* Content colors */
  --lcms-text-color: #333333;
  --lcms-background-color: #ffffff;
  --lcms-background-alt-color: #f8f9fa;
  --lcms-link-color: #007BFF;
  --lcms-muted-color: #6c757d;
  --lcms-border-color: #dee2e6;

  /* Typography */
  --lcms-font-heading: 'Playfair Display', serif;
  --lcms-font-body: 'Inter', sans-serif;
  --lcms-font-size-base: 16px;
  --lcms-line-height: 1.6;

  /* Layout */
  --lcms-border-radius: 8px;
  --lcms-container-max-width: 1200px;
  --lcms-section-gap: 40px;

  /* Buttons */
  --lcms-btn-padding: 10px 24px;
  --lcms-btn-border-radius: 6px;
  --lcms-btn-font-size: 14px;
  --lcms-btn-font-weight: 500;

  /* Links */
  --lcms-link-hover-color: #0056b3;
  --lcms-link-text-decoration: none;
  --lcms-link-hover-text-decoration: underline;
}

body {
  font-family: var(--lcms-font-body);
  font-size: var(--lcms-font-size-base);
  line-height: var(--lcms-line-height);
  color: var(--lcms-text-color);
  background-color: var(--lcms-background-color);
}

h1 { font-size: var(--lcms-h1-font-size, 2.5rem); font-weight: var(--lcms-h1-font-weight, 700); color: var(--lcms-h1-color, inherit); }
h2 { font-size: var(--lcms-h2-font-size, 2rem); font-weight: var(--lcms-h2-font-weight, 600); }
h3 { font-size: var(--lcms-h3-font-size, 1.75rem); font-weight: var(--lcms-h3-font-weight, 600); }

h1, h2, h3, h4, h5, h6 {
  font-family: var(--lcms-font-heading);
}

a {
  color: var(--lcms-link-color);
  text-decoration: var(--lcms-link-text-decoration);
}
a:hover {
  color: var(--lcms-link-hover-color);
  text-decoration: var(--lcms-link-hover-text-decoration);
}

.lcms-button {
  padding: var(--lcms-btn-padding);
  border-radius: var(--lcms-btn-border-radius);
  font-size: var(--lcms-btn-font-size);
  font-weight: var(--lcms-btn-font-weight);
}

.lcms-container {
  max-width: var(--lcms-container-max-width);
  margin: 0 auto;
}

.lcms-section + .lcms-section {
  margin-top: var(--lcms-section-gap);
}
```

---

## Styles Object Fields

All style fields are optional. The `styles` object only contains fields that have been configured — unconfigured fields will be absent.

### Theme Colors

| Field | Type | Description |
|-------|------|-------------|
| `primary_color` | string | Primary brand color (hex, e.g., `#007BFF`) |
| `secondary_color` | string | Secondary brand color (hex, e.g., `#6c757d`) |
| `accent_color` | string | Accent/highlight color (hex, e.g., `#FF6B35`) |

### Semantic Colors

| Field | Type | Description |
|-------|------|-------------|
| `success_color` | string | Success actions (hex, e.g., `#28a745`) |
| `danger_color` | string | Danger/error actions (hex, e.g., `#dc3545`) |
| `warning_color` | string | Warning/caution (hex, e.g., `#ffc107`) |
| `info_color` | string | Informational elements (hex, e.g., `#50a5f1`) |

### Neutral Colors

| Field | Type | Description |
|-------|------|-------------|
| `light_color` | string | Light background (hex, e.g., `#f8f9fa`) |
| `dark_color` | string | Dark background (hex, e.g., `#343a40`) |
| `white_color` | string | White (hex, e.g., `#ffffff`) |
| `black_color` | string | Black (hex, e.g., `#000000`) |

### Content Colors

| Field | Type | Description |
|-------|------|-------------|
| `text_color` | string | Default text color (hex, e.g., `#333333`) |
| `background_color` | string | Page background color (hex, e.g., `#ffffff`) |
| `background_alt_color` | string | Alternate background for sections (hex, e.g., `#f8f9fa`) |
| `link_color` | string | Link color (hex, e.g., `#007BFF`) |
| `muted_color` | string | Muted/secondary text (hex, e.g., `#6c757d`) |
| `border_color` | string | Default border color (hex, e.g., `#dee2e6`) |

### Custom Colors

| Field | Type | Description |
|-------|------|-------------|
| `custom_colors` | array | User-defined color palette |
| `custom_colors[].name` | string | Color label (e.g., `"Brand Gold"`) |
| `custom_colors[].color` | string | Color value (hex, e.g., `#c9a227`) |

### Typography — Base

| Field | Type | Description |
|-------|------|-------------|
| `font_heading` | string | Google Font family for headings (e.g., `Playfair Display`) |
| `font_body` | string | Google Font family for body text (e.g., `Inter`) |
| `font_size_base` | number | Base font size in pixels (12-24, e.g., `16`) |
| `line_height` | number | Base line height multiplier (1-2.5, e.g., `1.6`) |

### Typography — Headings (H1-H6)

Each heading level can be individually styled. Fields follow the pattern `h{n}_*`:

| Field | Type | Description |
|-------|------|-------------|
| `h1_font_size` | string | H1 font size (any CSS unit: `px`, `rem`, `clamp()`, etc.) |
| `h1_font_weight` | number | H1 font weight (300-900) |
| `h1_color` | string | H1 color (hex) |
| `h2_font_size` | string | H2 font size |
| `h2_font_weight` | number | H2 font weight |
| `h2_color` | string | H2 color |
| `h3_font_size` | string | H3 font size |
| `h3_font_weight` | number | H3 font weight |
| `h3_color` | string | H3 color |
| `h4_font_size` | string | H4 font size |
| `h4_font_weight` | number | H4 font weight |
| `h4_color` | string | H4 color |
| `h5_font_size` | string | H5 font size |
| `h5_font_weight` | number | H5 font weight |
| `h5_color` | string | H5 color |
| `h6_font_size` | string | H6 font size |
| `h6_font_weight` | number | H6 font weight |
| `h6_color` | string | H6 color |

### Typography — Paragraphs

| Field | Type | Description |
|-------|------|-------------|
| `p_font_size` | string | Paragraph font size (any CSS unit) |
| `p_font_weight` | number | Paragraph font weight (300-900) |
| `p_color` | string | Paragraph color (hex) |
| `p_line_height` | string | Paragraph line height (e.g., `"1.6"`) |

### Layout

| Field | Type | Description |
|-------|------|-------------|
| `border_radius` | number | Default border radius in pixels (0-50) |
| `container_max_width` | number | Maximum container width in pixels (800-1920) |
| `section_gap` | string | Default vertical gap between sections (e.g., `"40px"`) |

### Button Defaults

| Field | Type | Description |
|-------|------|-------------|
| `btn_padding` | string | Default button padding (e.g., `"10px 24px"`) |
| `btn_border_radius` | string | Default button border radius (e.g., `"6px"`) |
| `btn_font_size` | string | Default button font size (e.g., `"14px"`) |
| `btn_font_weight` | number | Default button font weight (300-900) |

### Link Defaults

| Field | Type | Description |
|-------|------|-------------|
| `link_hover_color` | string | Link hover color (hex, e.g., `#0056b3`) |
| `link_text_decoration` | string | Link text decoration: `"none"` or `"underline"` |
| `link_hover_text_decoration` | string | Link hover text decoration: `"none"` or `"underline"` |

---

## Color Variables

The `color_variables` array provides a resolved list of theme colors for use as CSS variables. These correspond to the `var:*` color references used throughout the page builder.

| Field | Type | Description |
|-------|------|-------------|
| `color_variables[].code` | string | Variable code (e.g., `"primary"`, `"accent"`) |
| `color_variables[].label` | string | Display label (e.g., `"Primary"`) |
| `color_variables[].value` | string | Resolved hex color value |

Default color variables (8):

| Code | Default Value |
|------|---------------|
| `primary` | `#007BFF` |
| `secondary` | `#6c757d` |
| `success` | `#28a745` |
| `danger` | `#dc3545` |
| `warning` | `#ffc107` |
| `info` | `#17a2b8` |
| `light` | `#f8f9fa` |
| `dark` | `#343a40` |

> **Note:** Values are overridden by the project's color settings. Widget configs may reference colors as `"var:primary"` — resolve them using this array.
