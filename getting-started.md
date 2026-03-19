# Getting Started

This guide will help you make your first API request in minutes.

## Prerequisites

Before you begin, you'll need:

- A LessCMS account with at least one workspace
- A project with published content
- An API key (we'll show you how to get one)

## Step 1: Getting Your API Key

API keys are generated from your LessCMS dashboard.

### Generate an API Key

1. Log in to your LessCMS dashboard
2. Navigate to your project settings
3. Go to **API Keys** section
4. Click **Generate New API Key**
5. Give your key a descriptive name (e.g., "Production Website")
6. Copy the generated key - **you won't be able to see it again**

> **Security Note:** Treat API keys like passwords. Never commit them to version control or expose them in client-side code.

### Finding Your Workspace and Project Codes

You'll need these codes for API requests:

- **Workspace Code**: Found in your workspace settings (e.g., `workspace123`)
- **Project Code**: Found in your project settings (e.g., `my-website`)

## Step 2: Your First Request

Let's fetch all pages from your project.

### Using cURL

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace123/project456/pages
```

### Using JavaScript (fetch)

```javascript
const response = await fetch(
  'https://api.lesscms.com/v1/workspace123/project456/pages',
  {
    headers: {
      'x-api-key': 'YOUR_API_KEY'
    }
  }
);

const data = await response.json();
console.log(data.pages);
```

### Using PHP

```php
<?php
$ch = curl_init();

curl_setopt($ch, CURLOPT_URL, 'https://api.lesscms.com/v1/workspace123/project456/pages');
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_HTTPHEADER, [
    'x-api-key: YOUR_API_KEY'
]);

$response = curl_exec($ch);
curl_close($ch);

$data = json_decode($response, true);
print_r($data['pages']);
?>
```

### Using Python

```python
import requests

response = requests.get(
    'https://api.lesscms.com/v1/workspace123/project456/pages',
    headers={'x-api-key': 'YOUR_API_KEY'}
)

data = response.json()
print(data['pages'])
```

### Expected Response

```json
{
  "pages": [
    {
      "code": "homepage",
      "name": "Homepage",
      "is_public": true,
      "in_sitemap": true,
      "custom_route": "/",
      "content": {
        "title": {
          "en": "Welcome",
          "pl": "Witamy"
        },
        "hero_text": {
          "en": "Build amazing websites",
          "pl": "Twórz niesamowite strony"
        }
      }
    }
  ]
}
```

## Step 3: Loading Project Styles

Before rendering content, fetch your project's style configuration. The `/config` endpoint returns all visual settings configured in the CMS dashboard -- colors, fonts, typography, layout, and more. These styles are the foundation that widgets reference.

### Fetch Config

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace123/project456/config
```

### Key Fields in the Response

| Field | Description |
|-------|-------------|
| `styles` | All visual settings (colors, typography, buttons, inputs, links, layout) |
| `color_variables` | Array of `{ code, label, value }` objects -- maps color names to resolved hex values |
| `google_fonts_url` | Ready-to-use URL for a `<link>` tag to load Google Fonts |
| `fonts` | Array of Google Font family names enabled for the project |
| `languages` | Array of language codes enabled for the project |
| `default_language` | Default language code |

### Setting Up Styles in Your App

```javascript
async function loadConfig() {
  const response = await fetch(
    'https://api.lesscms.com/v1/workspace/project/config',
    { headers: { 'x-api-key': API_KEY } }
  );
  const { data } = await response.json();

  // Load Google Fonts
  if (data.google_fonts_url) {
    const link = document.createElement('link');
    link.href = data.google_fonts_url;
    link.rel = 'stylesheet';
    document.head.appendChild(link);
  }

  // Set CSS variables from color_variables
  data.color_variables.forEach(cv => {
    document.documentElement.style.setProperty(`--lcms-color-${cv.code}`, cv.value);
  });

  // Set typography variables
  const s = data.styles;
  if (s.font_heading) document.documentElement.style.setProperty('--lcms-font-heading', s.font_heading);
  if (s.font_body) document.documentElement.style.setProperty('--lcms-font-body', s.font_body);
  if (s.border_radius) document.documentElement.style.setProperty('--lcms-border-radius', s.border_radius + 'px');

  return data;
}
```

### Understanding Color Variables

Widgets in the page builder use `var:colorName` syntax to reference project colors. For example, a button widget might have `background: "var:primary"`. The `color_variables` array maps these names to actual values:

```json
{
  "color_variables": [
    { "code": "primary", "label": "Primary", "value": "#007BFF" },
    { "code": "secondary", "label": "Secondary", "value": "#6c757d" },
    { "code": "accent", "label": "Accent", "value": "#FF6B35" },
    { "code": "success", "label": "Success", "value": "#28a745" },
    { "code": "danger", "label": "Danger", "value": "#dc3545" },
    { "code": "warning", "label": "Warning", "value": "#ffc107" },
    { "code": "info", "label": "Info", "value": "#17a2b8" },
    { "code": "light", "label": "Light", "value": "#f8f9fa" },
    { "code": "dark", "label": "Dark", "value": "#343a40" },
    { "code": "white", "label": "White", "value": "#ffffff" },
    { "code": "black", "label": "Black", "value": "#000000" },
    { "code": "text", "label": "Text", "value": "#212529" },
    { "code": "text-muted", "label": "Text Muted", "value": "#6c757d" },
    { "code": "link", "label": "Link", "value": "#007BFF" },
    { "code": "background", "label": "Background", "value": "#ffffff" },
    { "code": "background-alt", "label": "Background Alt", "value": "#f8f9fa" },
    { "code": "border", "label": "Border", "value": "#dee2e6" }
  ]
}
```

When rendering a widget with `"color": "var:primary"`, resolve it by looking up `primary` in the `color_variables` array to get `#007BFF`. Custom colors defined in the CMS appear with a `custom-` prefix (e.g., `custom-brand-red`).

### Style Properties Reference

The `styles` object contains all visual settings for the project. Key categories:

**Colors:** `primary_color`, `secondary_color`, `accent_color`, `text_color`, `background_color`, `border_color`, plus status colors (`success_color`, `danger_color`, `warning_color`, `info_color`) and neutral colors (`light_color`, `dark_color`, `white_color`, `black_color`, `muted_color`, `background_alt_color`, `link_color`). A `custom_colors` array holds additional project-specific colors.

**Typography:** `font_heading`, `font_body`, `font_size_base`, `line_height`. Heading sizes and weights are available per level: `h1_font_size`, `h1_font_weight`, `h1_color` through `h6_*`. Paragraph defaults: `p_font_size`, `p_font_weight`, `p_color`.

**Buttons:** `btn_padding`, `btn_border_radius`, `btn_font_size`, `btn_font_weight`.

**Inputs:** `input_background_color`, `input_text_color`, `input_border_color`, `input_focus_border_color`, `input_placeholder_color`.

**Links:** `link_color`, `link_hover_color`, `link_text_decoration`, `link_hover_text_decoration`.

**Layout:** `border_radius`, `container_max_width`.

These styles apply globally. Fonts from `font_heading` and `font_body` are loaded via the `google_fonts_url`. Widgets inherit these defaults and can override them individually.

[Learn more about Config →](reference/config.md)

---

## Step 4: Filtering Content

### Get a Specific Page

Fetch a single page by its code:

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace123/project456/pages/homepage
```

### Get Content in a Specific Language

Use the `language` query parameter:

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  "https://api.lesscms.com/v1/workspace123/project456/pages/homepage?language=pl"
```

Response with Polish content:

```json
{
  "code": "homepage",
  "name": "Homepage",
  "content": {
    "title": "Witamy",
    "hero_text": "Twórz niesamowite strony"
  }
}
```

## Step 5: Working with Collections

Collections are perfect for structured content like blog posts, products, or team members.

### List All Entries

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace123/project456/collections/blog-posts
```

### Get a Single Entry

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace123/project456/collections/blog-posts/my-first-post
```

### Filter and Sort

Collections support powerful filtering:

```bash
# Filter by field value
curl -H "x-api-key: YOUR_API_KEY" \
  "https://api.lesscms.com/v1/workspace123/project456/collections/blog-posts?filter[category]=technology"

# Sort results
curl -H "x-api-key: YOUR_API_KEY" \
  "https://api.lesscms.com/v1/workspace123/project456/collections/blog-posts?sort=-published_date"

# Limit results
curl -H "x-api-key: YOUR_API_KEY" \
  "https://api.lesscms.com/v1/workspace123/project456/collections/blog-posts?limit=10"
```

[Learn more about Collections →](reference/collections.md)

## Step 6: Fetching Menus

Menus provide navigation structures for your site:

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace123/project456/menus/main-navigation
```

Response:

```json
{
  "code": "main-navigation",
  "name": "Main Navigation",
  "items": [
    {
      "label": { "en": "Home", "pl": "Strona główna" },
      "url": "/",
      "target": "_self",
      "children": []
    },
    {
      "label": { "en": "About", "pl": "O nas" },
      "url": "/about",
      "target": "_self",
      "children": [
        {
          "label": { "en": "Team", "pl": "Zespół" },
          "url": "/about/team",
          "target": "_self"
        }
      ]
    }
  ]
}
```

[Learn more about Menus →](reference/menus.md)

## Common Use Cases

### Building a Static Site

```javascript
// Fetch all pages during build
async function getAllPages() {
  const response = await fetch(
    'https://api.lesscms.com/v1/workspace/project/pages',
    {
      headers: { 'x-api-key': process.env.LESSCMS_API_KEY }
    }
  );

  const { pages } = await response.json();

  // Generate static pages
  for (const page of pages) {
    await generatePage(page);
  }
}
```

### Building a Blog

```javascript
// Fetch recent blog posts
async function getRecentPosts(limit = 10) {
  const response = await fetch(
    `https://api.lesscms.com/v1/workspace/project/collections/blog-posts?sort=-published_date&limit=${limit}`,
    {
      headers: { 'x-api-key': process.env.LESSCMS_API_KEY }
    }
  );

  const { entries } = await response.json();
  return entries;
}
```

### Implementing Dynamic Routing

```javascript
// Get route configuration
async function getRoutes() {
  const response = await fetch(
    'https://api.lesscms.com/v1/workspace/project/routes',
    {
      headers: { 'x-api-key': process.env.LESSCMS_API_KEY }
    }
  );

  const { routes } = await response.json();

  // Map routes to your framework
  routes.forEach(route => {
    router.add(route.path, route.handler);
  });
}
```

## Best Practices

### 1. Cache Responses

The API includes `Cache-Tag` headers for efficient caching:

```javascript
// Cache API responses
const response = await fetch(url, { headers });
const cacheTag = response.headers.get('Cache-Tag');

// Store in your cache with the tag
cache.set(url, data, { tags: [cacheTag] });

// Later, invalidate by tag when content changes
cache.invalidate(cacheTag);
```

### 2. Handle Errors Gracefully

```javascript
async function fetchContent(url) {
  try {
    const response = await fetch(url, { headers });

    if (!response.ok) {
      throw new Error(`HTTP ${response.status}: ${response.statusText}`);
    }

    return await response.json();
  } catch (error) {
    console.error('Failed to fetch content:', error);

    // Return fallback content or handle error
    return null;
  }
}
```

### 3. Use Environment Variables

Never hardcode API keys:

```bash
# .env file
LESSCMS_API_KEY=your_api_key_here
LESSCMS_WORKSPACE=workspace123
LESSCMS_PROJECT=project456
```

```javascript
const API_KEY = process.env.LESSCMS_API_KEY;
const WORKSPACE = process.env.LESSCMS_WORKSPACE;
const PROJECT = process.env.LESSCMS_PROJECT;

const baseUrl = `https://api.lesscms.com/v1/${WORKSPACE}/${PROJECT}`;
```

## Next Steps

Now that you've made your first API request, explore more:

- [Authentication Guide](guides/authentication.md) - Security best practices
- [API Reference](reference/) - Complete endpoint documentation
- [Error Handling](guides/errors.md) - Understanding error responses
- [Caching Strategy](guides/caching.md) - Optimize performance

## Need Help?

- Check the [API Reference](reference/) for detailed endpoint documentation
- Browse [Guides](guides/) for advanced topics
- Join our [Community](https://community.lesscms.com)
- Report issues on [GitHub](https://github.com/lesscms/api/issues)
