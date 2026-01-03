# Pages

Pages represent individual web pages with dynamic content sections. Perfect for homepages, about pages, contact forms, and any standalone page.

## Endpoints

<span class="endpoint-badge get">GET</span> `/v1/:workspace_code/:project_code/pages`

<span class="endpoint-badge get">GET</span> `/v1/:workspace_code/:project_code/pages/:code`

<span class="endpoint-badge get">GET</span> `/v1/:workspace_code/:project_code/pages/:code/content/:scope/:mode?`

<span class="endpoint-badge get">GET</span> `/v1/:workspace_code/:project_code/pages/preview/:preview_token`

---

## List All Pages

Get all public pages in a project.

```
GET /v1/:workspace_code/:project_code/pages
```

### Example Request

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace123/project456/pages
```

### Example Response

```json
{
  "data": [
    {
      "page_uuid": "550e8400-e29b-41d4-a716-446655440000",
      "code": "homepage",
      "name": "Homepage",
      "name_translation": {
        "en": "Homepage",
        "pl": "Strona główna"
      },
      "description": "Main landing page",
      "description_translation": {
        "en": "Main landing page",
        "pl": "Główna strona docelowa"
      },
      "schema_name": "Homepage Template",
      "schema_code": "homepage-template",
      "schema_name_translation": {
        "en": "Homepage Template",
        "pl": "Szablon strony głównej"
      },
      "sort_order": 1,
      "created_at": "2024-01-15T10:00:00Z",
      "updated_at": "2024-12-15T14:30:00Z"
    },
    {
      "page_uuid": "660e8400-e29b-41d4-a716-446655440001",
      "code": "about",
      "name": "About Us",
      "name_translation": {
        "en": "About Us",
        "pl": "O nas"
      },
      "description": "Company information",
      "schema_name": "Content Page",
      "schema_code": "content-page",
      "sort_order": 2,
      "created_at": "2024-01-15T10:00:00Z",
      "updated_at": "2024-12-10T09:15:00Z"
    }
  ]
}
```

### Response Headers

```
Cache-Tag: project-{project_uuid} project-{project_uuid}-pages
```

---

## Get Single Page

Retrieve a specific page by its code with full content.

```
GET /v1/:workspace_code/:project_code/pages/:code
```

### Path Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `code` | string | **Required.** Page code (e.g., `homepage`, `about`) |

### Example Request

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace123/project456/pages/homepage
```

### Example Response

```json
{
  "data": {
    "content": [
      {
        "uuid": "section-1",
        "columns_count": 1,
        "order": 0,
        "settings": {
          "paddingTop": 60,
          "paddingBottom": 60,
          "backgroundColor": "#f5f5f5"
        },
        "columns": [
          {
            "uuid": "column-1",
            "settings": {
              "verticalAlign": "center",
              "horizontalAlign": "center"
            },
            "content": [
              {
                "widget_type": "heading",
                "uuid": "widget-1",
                "content": {
                  "html": {
                    "en": "Welcome to Our Site",
                    "pl": "Witamy na naszej stronie"
                  }
                },
                "config": {
                  "level": 1,
                  "textAlign": "center"
                }
              }
            ]
          }
        ]
      }
    ],
    "metadata": {
      "code": "homepage",
      "page_uuid": "550e8400-e29b-41d4-a716-446655440000",
      "schema_name": "Homepage Template",
      "schema_code": "homepage-template",
      "schema_name_translation": {
        "en": "Homepage Template",
        "pl": "Szablon strony głównej"
      },
      "is_public": true,
      "in_sitemap": true,
      "created_at": "2024-01-15T10:00:00Z",
      "updated_at": "2024-12-15T14:30:00Z",
      "url": "/"
    },
    "seo": {
      "title": {
        "en": "Homepage - My Website",
        "pl": "Strona główna - Moja Witryna"
      },
      "description": {
        "en": "Welcome to our website",
        "pl": "Witamy na naszej stronie"
      },
      "keywords": ["homepage", "landing", "welcome"],
      "og_image": "https://cdn.example.com/og-homepage.jpg",
      "canonical_url": "https://example.com/"
    }
  }
}
```

### Response Structure

The response contains three main sections:

#### `content`

Array of sections with columns and widgets. Schema sections and custom sections are merged and sorted by `order`.

#### `metadata`

| Field | Type | Description |
|-------|------|-------------|
| `code` | string | Unique page identifier |
| `page_uuid` | string | Page UUID |
| `schema_name` | string | Name of the page schema template |
| `schema_code` | string | Code of the page schema |
| `schema_name_translation` | object | Multilingual schema name |
| `is_public` | boolean | Whether page is published (default: `true`) |
| `in_sitemap` | boolean | Whether page appears in sitemap (default: `true`) |
| `created_at` | string | ISO 8601 creation timestamp |
| `updated_at` | string | ISO 8601 last update timestamp |
| `url` | string | Full URL path (custom_route or auto-generated) |

#### `seo` (optional)

| Field | Type | Description |
|-------|------|-------------|
| `title` | object | Multilingual page title for SEO |
| `description` | object | Multilingual meta description |
| `keywords` | array | SEO keywords |
| `og_image` | string | Open Graph image URL |
| `canonical_url` | string | Canonical URL for duplicate content |

### Response Headers

```
Cache-Tag: project-{project_uuid} project-{project_uuid}-pages
```

### Error Response

If page not found:

```json
{
  "error": "Page not found",
  "message": "Page with code 'nonexistent' not found"
}
```

HTTP Status: `404 Not Found`

---

## Get Page Content by Scope

Retrieve page content filtered by scope (schema fields or custom sections).

```
GET /v1/:workspace_code/:project_code/pages/:code/content/:scope/:mode?
```

### Path Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `code` | string | **Required.** Page code |
| `scope` | string | **Required.** Content scope: `schema` or `custom` |
| `mode` | string | Optional. For scope=schema only: `flat` returns key-value pairs |

### Scope Values

| Scope | Description |
|-------|-------------|
| `schema` | Returns content from schema-defined sections (fields from page template) |
| `custom` | Returns content from custom sections added via page builder |

### Mode Values (for scope=schema only)

| Mode | Description |
|------|-------------|
| (none) | Returns full section structure with columns and widgets |
| `flat` | Returns flattened object with `field_code: value` pairs |

### Example: Schema Content (Structured)

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace/project/pages/homepage/content/schema
```

```json
{
  "data": {
    "content": [
      {
        "uuid": "section-1",
        "columns_count": 1,
        "columns": [
          {
            "uuid": "column-1",
            "content": [
              {
                "field_code": "title",
                "value": {
                  "en": "Welcome",
                  "pl": "Witamy"
                }
              }
            ]
          }
        ]
      }
    ],
    "metadata": { /* ... */ },
    "seo": { /* ... */ }
  }
}
```

### Example: Schema Content (Flat)

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace/project/pages/homepage/content/schema/flat
```

```json
{
  "data": {
    "content": {
      "title": {
        "en": "Welcome",
        "pl": "Witamy"
      },
      "subtitle": {
        "en": "Discover our services",
        "pl": "Odkryj nasze usługi"
      },
      "featured_image": "https://cdn.example.com/hero.jpg"
    },
    "metadata": { /* ... */ },
    "seo": { /* ... */ }
  }
}
```

### Example: Custom Content

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace/project/pages/homepage/content/custom
```

```json
{
  "data": {
    "content": [
      {
        "uuid": "custom-section-1",
        "columns_count": 2,
        "is_custom": true,
        "order": 10,
        "columns": [
          {
            "uuid": "column-1",
            "content": [
              {
                "widget_type": "text",
                "uuid": "widget-1",
                "content": {
                  "html": {
                    "en": "<p>Custom content here</p>"
                  }
                }
              }
            ]
          }
        ]
      }
    ],
    "metadata": { /* ... */ }
  }
}
```

---

## Preview Page (Draft)

Get a preview of a page draft using a preview token.

```
GET /v1/:workspace_code/:project_code/pages/preview/:preview_token
```

### Path Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `preview_token` | string | **Required.** Preview token generated in CMS |

### Example Request

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace/project/pages/preview/abc123xyz789
```

### Example Response

```json
{
  "data": {
    "sections": [ /* same structure as content */ ],
    "code": "homepage",
    "page_uuid": "550e8400-e29b-41d4-a716-446655440000",
    "created_at": "2024-01-15T10:00:00Z",
    "updated_at": "2024-12-15T14:30:00Z",
    "metadata": { /* ... */ },
    "schema_name": "Homepage Template",
    "schema_code": "homepage-template",
    "schema_name_translation": { /* ... */ },
    "is_draft": true
  }
}
```

**Note:** Draft previews are not cached.

---

## Content Structure

Pages use a hierarchical content structure:

```
Page
└── Sections (array)
    └── Columns (array)
        └── Content/Widgets (array)
```

### Section

A horizontal section of the page:

```json
{
  "uuid": "section-1",
  "columns_count": 2,
  "order": 0,
  "is_custom": false,
  "settings": {
    "paddingTop": 40,
    "paddingBottom": 40,
    "backgroundColor": "#f5f5f5",
    "backgroundOpacity": 100,
    "backgroundImage": "https://cdn.example.com/bg.jpg",
    "backgroundSize": "cover",
    "backgroundPosition": "center center",
    "contentWidth": "1200px",
    "sectionHeight": 500,
    "fullHeight": false,
    "stackOnTablet": false,
    "stackOnMobile": true,
    "gradient": {
      "type": "linear",
      "angle": 180,
      "colorStart": "#ffffff",
      "colorEnd": "#f0f0f0"
    },
    "responsive": {
      "mobile": { "hidden": true }
    }
  },
  "columns": [ /* array of columns */ ]
}
```

### Column

A vertical column within a section:

```json
{
  "uuid": "column-1",
  "width": 6,
  "settings": {
    "paddingLeft": 15,
    "paddingRight": 15,
    "backgroundColor": "#ffffff",
    "columnHeight": 400,
    "minHeight": 200,
    "verticalAlign": "center",
    "horizontalAlign": "flex-start",
    "responsive": {
      "tablet": { "hidden": false },
      "mobile": { "hidden": false }
    }
  },
  "content": [ /* array of widgets */ ]
}
```

### Widget

Content elements within a column:

```json
{
  "widget_type": "heading",
  "uuid": "widget-1",
  "content": {
    "html": {
      "en": "Welcome",
      "pl": "Witamy"
    }
  },
  "config": {
    "level": 1
  },
  "settings": {
    "paddingTop": 10,
    "paddingBottom": 20,
    "marginBottom": 30,
    "backgroundColor": "#f0f0f0",
    "textAlign": "center",
    "cssClass": "hero-heading",
    "responsive": {
      "tablet": {
        "paddingTop": 5,
        "paddingBottom": 10
      },
      "mobile": {
        "paddingTop": 0,
        "paddingBottom": 5,
        "hidden": false
      }
    }
  }
}
```

---

## Widget Types

Common widget types:

- `heading` - Text headings (h1-h6)
- `text` - Text paragraphs
- `text-rich-html` - Rich HTML content
- `image` - Single images
- `gallery` - Image galleries
- `button` - Call-to-action buttons
- `video` - Embedded videos
- `spacer` - Vertical spacing
- `divider` - Horizontal lines
- `hero` - Hero sections
- `alert` - Alert boxes
- `blockquote` - Quotes
- `icon-box` - Icon with text
- `social-icons` - Social media links
- `countdown` - Countdown timers
- `collection-grid` - Collection entries grid
- `menu` - Navigation menu

---

## Common Widget Settings

All widgets can include a `settings` object with common style properties. These settings support responsive overrides for tablet and mobile breakpoints.

### Spacing Properties

| Property | Type | Description |
|----------|------|-------------|
| `paddingTop` | number | Top padding in pixels |
| `paddingRight` | number | Right padding in pixels |
| `paddingBottom` | number | Bottom padding in pixels |
| `paddingLeft` | number | Left padding in pixels |
| `marginTop` | number | Top margin in pixels |
| `marginRight` | number | Right margin in pixels |
| `marginBottom` | number | Bottom margin in pixels |
| `marginLeft` | number | Left margin in pixels |

### Background Properties

| Property | Type | Description |
|----------|------|-------------|
| `backgroundColor` | string | Background color (hex) |
| `backgroundOpacity` | number | Background opacity (0-100) |
| `backgroundImage` | string | Background image URL |
| `backgroundSize` | string | Background size: `cover`, `contain`, `auto` |
| `backgroundPosition` | string | Background position (e.g., `center center`) |

### Border Properties

| Property | Type | Description |
|----------|------|-------------|
| `borderRadius` | number | Border radius in pixels |
| `borderWidth` | number | Border width in pixels |
| `borderColor` | string | Border color (hex) |
| `borderStyle` | string | Border style: `solid`, `dashed`, `dotted` |
| `boxShadow` | string | CSS box-shadow value |

### Size Properties

| Property | Type | Description |
|----------|------|-------------|
| `width` | string/number | Width (pixels or percentage) |
| `maxWidth` | string/number | Maximum width |
| `minWidth` | string/number | Minimum width |
| `height` | number | Height in pixels |
| `maxHeight` | number | Maximum height |
| `minHeight` | number | Minimum height |

### Alignment Properties

| Property | Type | Description |
|----------|------|-------------|
| `textAlign` | string | Text alignment: `left`, `center`, `right` |
| `verticalAlign` | string | Vertical alignment: `flex-start`, `center`, `flex-end` |
| `horizontalAlign` | string | Horizontal alignment: `flex-start`, `center`, `flex-end`, `stretch` |

### Custom Styling

| Property | Type | Description |
|----------|------|-------------|
| `cssClass` | string | Custom CSS class name |
| `customCss` | string | Custom CSS rules |

### Responsive Overrides

Widget settings can include responsive overrides for tablet (768-1199px) and mobile (0-767px) breakpoints:

```json
{
  "settings": {
    "paddingTop": 40,
    "paddingBottom": 40,
    "responsive": {
      "tablet": {
        "paddingTop": 20,
        "paddingBottom": 20
      },
      "mobile": {
        "paddingTop": 10,
        "paddingBottom": 10,
        "hidden": true
      }
    }
  }
}
```

**Fallback logic:**
- Desktop: Uses top-level property values
- Tablet: Uses `responsive.tablet` overrides, falls back to desktop values
- Mobile: Uses `responsive.mobile` overrides, falls back to desktop values

**Note:** The `hidden` property is independent per breakpoint - it does not fall back to desktop. This allows showing different content on different devices.

---

## Use Cases

### Rendering a Page

```javascript
async function renderPage(pageCode) {
  const response = await fetch(
    `https://api.lesscms.com/v1/workspace/project/pages/${pageCode}`,
    {
      headers: { 'x-api-key': API_KEY }
    }
  );

  const { data } = await response.json();

  // Render SEO meta tags
  if (data.seo) {
    document.title = data.seo.title.en;
    document.querySelector('meta[name="description"]').content = data.seo.description.en;
  }

  // Render page content
  data.content.forEach(section => {
    section.columns.forEach(column => {
      column.content.forEach(widget => {
        renderWidget(widget);
      });
    });
  });
}
```

### Get Schema Fields Only (Flat)

```javascript
async function getPageFields(pageCode) {
  const response = await fetch(
    `https://api.lesscms.com/v1/workspace/project/pages/${pageCode}/content/schema/flat`,
    {
      headers: { 'x-api-key': API_KEY }
    }
  );

  const { data } = await response.json();

  // Easy access to field values
  const title = data.content.title?.en;
  const heroImage = data.content.hero_image;

  return data.content;
}
```

### Static Site Generation

```javascript
async function generateStaticSite() {
  const response = await fetch(
    'https://api.lesscms.com/v1/workspace/project/pages',
    {
      headers: { 'x-api-key': process.env.LESSCMS_API_KEY }
    }
  );

  const { data: pages } = await response.json();

  for (const pageMeta of pages) {
    const fullPage = await fetch(
      `https://api.lesscms.com/v1/workspace/project/pages/${pageMeta.code}`,
      {
        headers: { 'x-api-key': process.env.LESSCMS_API_KEY }
      }
    ).then(r => r.json());

    await generateHTML(fullPage.data);
  }
}
```

---

## Related Resources

- [Collections](collections.md)
- [Blocks](blocks.md)
- [Menus](menus.md)
- [Routes](routes.md)
- [Sitemap](sitemap.md)
