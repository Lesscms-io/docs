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
      "text_color": "#333333",
      "background_color": "#ffffff",
      "link_color": "#007BFF",
      "font_heading": "Playfair Display",
      "font_body": "Inter",
      "font_size_base": 16,
      "line_height": 1.6,
      "border_radius": 8,
      "container_max_width": 1200
    },
    "fonts": ["Inter", "Roboto"],
    "custom_css_urls": ["https://cdn.example.com/custom.css"],
    "available_widgets": [
      "text",
      "text-rich-html",
      "heading",
      "image",
      "gallery",
      "button",
      "icon",
      "divider",
      "spacer",
      "blockquote",
      "icon-list",
      "video",
      "image-carousel",
      "hero",
      "toggle",
      "countdown",
      "counter",
      "progress-bar",
      "testimonial",
      "alert",
      "star-rating",
      "menu",
      "social-icons",
      "collection-grid",
      "collection-carousel",
      "collection-single"
    ],
    "available_fonts": [
      "Inter",
      "Roboto",
      "Open Sans",
      "Lato",
      "Montserrat",
      "Poppins",
      "Raleway",
      "Oswald",
      "Source Sans Pro",
      "Nunito",
      "Ubuntu",
      "Merriweather",
      "Playfair Display",
      "PT Sans",
      "Lora",
      "Noto Sans",
      "Fira Sans",
      "Work Sans",
      "Rubik",
      "Quicksand",
      "Mulish",
      "Barlow",
      "Karla",
      "Inconsolata",
      "Space Grotesk",
      "DM Sans",
      "Manrope",
      "Outfit",
      "Plus Jakarta Sans",
      "Lexend"
    ],
    "google_fonts_url": "https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&family=Roboto:wght@300;400;500;600;700&display=swap",
    "page_route_schema": "/p/{slug}",
    "collection_route_schema": "/c/{collection_code}/{slug}",
    "homepage_uuid": "550e8400-e29b-41d4-a716-446655440000"
  }
}
```

### Response Fields

| Field | Type | Description |
|-------|------|-------------|
| `styles` | object | Theme style settings (colors, typography, layout) |
| `fonts` | array | Google Font family names enabled for this project |
| `custom_css_urls` | array | URLs to external custom stylesheets |
| `available_widgets` | array | Widget type codes enabled for page builder |
| `available_fonts` | array | All available Google Fonts for font picker |
| `google_fonts_url` | string | Pre-built Google Fonts URL for `<link>` tag |
| `page_route_schema` | string | URL pattern for pages (e.g., `/p/{slug}`) |
| `collection_route_schema` | string | URL pattern for collection entries (e.g., `/c/{collection_code}/{slug}`) |
| `homepage_uuid` | string\|null | UUID of the page set as homepage (`null` if not set) |

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

config.data.custom_css_urls.forEach(url => {
  const link = document.createElement('link');
  link.rel = 'stylesheet';
  link.href = url;
  document.head.appendChild(link);
});
```

### Apply Theme Styles (CSS Variables)

```javascript
const config = await fetchConfig();
const { styles } = config.data;

// Apply CSS variables to :root
const root = document.documentElement;

if (styles.primary_color) {
  root.style.setProperty('--lcms-primary-color', styles.primary_color);
}
if (styles.secondary_color) {
  root.style.setProperty('--lcms-secondary-color', styles.secondary_color);
}
if (styles.text_color) {
  root.style.setProperty('--lcms-text-color', styles.text_color);
}
if (styles.background_color) {
  root.style.setProperty('--lcms-background-color', styles.background_color);
}
if (styles.link_color) {
  root.style.setProperty('--lcms-link-color', styles.link_color);
}
if (styles.font_heading) {
  root.style.setProperty('--lcms-font-heading', `'${styles.font_heading}', sans-serif`);
}
if (styles.font_body) {
  root.style.setProperty('--lcms-font-body', `'${styles.font_body}', sans-serif`);
}
if (styles.font_size_base) {
  root.style.setProperty('--lcms-font-size-base', `${styles.font_size_base}px`);
}
if (styles.line_height) {
  root.style.setProperty('--lcms-line-height', styles.line_height);
}
if (styles.border_radius !== undefined) {
  root.style.setProperty('--lcms-border-radius', `${styles.border_radius}px`);
}
if (styles.container_max_width) {
  root.style.setProperty('--lcms-container-max-width', `${styles.container_max_width}px`);
}
```

### Example CSS Using Variables

```css
/* lcms-styles.css */

:root {
  /* Fallback values - overridden by JavaScript from /config */
  --lcms-primary-color: #007BFF;
  --lcms-secondary-color: #6c757d;
  --lcms-text-color: #333333;
  --lcms-background-color: #ffffff;
  --lcms-link-color: #007BFF;
  --lcms-font-heading: 'Playfair Display', serif;
  --lcms-font-body: 'Inter', sans-serif;
  --lcms-font-size-base: 16px;
  --lcms-line-height: 1.6;
  --lcms-border-radius: 8px;
  --lcms-container-max-width: 1200px;
}

body {
  font-family: var(--lcms-font-body);
  font-size: var(--lcms-font-size-base);
  line-height: var(--lcms-line-height);
  color: var(--lcms-text-color);
  background-color: var(--lcms-background-color);
}

h1, h2, h3, h4, h5, h6 {
  font-family: var(--lcms-font-heading);
}

a {
  color: var(--lcms-link-color);
}

.lcms-button-primary {
  background-color: var(--lcms-primary-color);
  border-radius: var(--lcms-border-radius);
}

.lcms-button-secondary {
  background-color: var(--lcms-secondary-color);
  border-radius: var(--lcms-border-radius);
}

.lcms-container {
  max-width: var(--lcms-container-max-width);
  margin: 0 auto;
}
```

---

## Styles Object Fields

| Field | Type | Description |
|-------|------|-------------|
| `primary_color` | string | Primary action color (hex, e.g., `#007BFF`) |
| `secondary_color` | string | Secondary color (hex, e.g., `#6c757d`) |
| `text_color` | string | Default text color (hex, e.g., `#333333`) |
| `background_color` | string | Page background color (hex, e.g., `#ffffff`) |
| `link_color` | string | Link color (hex, e.g., `#007BFF`) |
| `font_heading` | string | Google Font family for headings (e.g., `Playfair Display`) |
| `font_body` | string | Google Font family for body text (e.g., `Inter`) |
| `font_size_base` | number | Base font size in pixels (12-24, e.g., `16`) |
| `line_height` | number | Line height multiplier (1-2.5, e.g., `1.6`) |
| `border_radius` | number | Border radius in pixels (0-50, e.g., `8`) |
| `container_max_width` | number | Maximum container width in pixels (800-1920, e.g., `1200`) |

> **Note:** All style fields are optional. Empty object `{}` is returned if no styles are configured.
