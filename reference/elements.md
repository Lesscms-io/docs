# Elements

Global page builder content accessible across your site. Elements are reusable content sections (e.g., contact info, newsletter signup, call-to-action) that can be embedded into any page.

Elements use the same section/column/widget structure as pages. They are resolved automatically when referenced inside page content -- if a page section has `grid_type: "element"`, the API replaces it with the actual element content.

## Endpoints

<span class="endpoint-badge get">GET</span> `/v1/:workspace_code/:project_code/elements`

<span class="endpoint-badge get">GET</span> `/v1/:workspace_code/:project_code/elements/:code`

<span class="endpoint-badge get">GET</span> `/v1/:workspace_code/:project_code/elements/preview/:preview_token`

---

## List All Elements

Get all public elements in a project.

```
GET /v1/:workspace_code/:project_code/elements
```

### Example Request

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace/project/elements
```

### Example Response

```json
{
  "data": [
    {
      "element_uuid": "650e8400-e29b-41d4-a716-446655440000",
      "code": "contact-info",
      "name": "Contact Information",
      "name_translation": {
        "en": "Contact Information",
        "pl": "Dane kontaktowe"
      },
      "sort_order": 1,
      "created_at": "2024-01-15T10:00:00Z",
      "updated_at": "2024-12-15T14:30:00Z",
      "data": {},
      "custom_sections": [
        {
          "uuid": "section-contact",
          "grid_type": "2-columns",
          "order": 0,
          "columns": [
            {
              "uuid": "col-c1",
              "span": 6,
              "content": [
                {
                  "uuid": "widget-c1",
                  "widget_type": "icon-list",
                  "data": {
                    "items": [
                      {
                        "icon": "fa-solid fa-envelope",
                        "text": { "en": "hello@example.com", "pl": "hello@example.com" }
                      },
                      {
                        "icon": "fa-solid fa-phone",
                        "text": { "en": "+1 234 567 890", "pl": "+1 234 567 890" }
                      }
                    ],
                    "style": {}
                  }
                }
              ]
            },
            {
              "uuid": "col-c2",
              "span": 6,
              "content": [
                {
                  "uuid": "widget-c2",
                  "widget_type": "text",
                  "data": {
                    "text": {
                      "en": "123 Main Street, City, Country",
                      "pl": "ul. Glowna 123, Miasto, Polska"
                    },
                    "style": {}
                  }
                }
              ]
            }
          ],
          "settings": {
            "padding_top": 40,
            "padding_bottom": 40
          }
        }
      ],
      "metadata": {
        "is_public": true
      }
    }
  ]
}
```

### Response Headers

```
Cache-Tag: project-{project_uuid} project-{project_uuid}-elements
```

---

## Get Single Element

Retrieve a specific element by its code. The response includes both structured content (sections with widgets) and a flattened version for simple field access.

```
GET /v1/:workspace_code/:project_code/elements/:code
```

### Path Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `code` | string | **Required.** Element code (e.g., `contact-info`, `newsletter`) |

### Example Request

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace/project/elements/contact-info
```

### Example Response

```json
{
  "data": {
    "content": [
      {
        "uuid": "section-contact",
        "columns_count": 2,
        "order": 0,
        "settings": {
          "paddingTop": 40,
          "paddingBottom": 40
        },
        "columns": [
          {
            "uuid": "col-c1",
            "settings": {},
            "widgets": [
              {
                "uuid": "widget-c1",
                "widget_type": "icon-list",
                "config": {
                  "items": [
                    {
                      "icon": "fa-solid fa-envelope",
                      "text": { "en": "hello@example.com", "pl": "hello@example.com" }
                    },
                    {
                      "icon": "fa-solid fa-phone",
                      "text": { "en": "+1 234 567 890", "pl": "+1 234 567 890" }
                    }
                  ]
                },
                "settings": {}
              }
            ]
          },
          {
            "uuid": "col-c2",
            "settings": {},
            "widgets": [
              {
                "uuid": "widget-c2",
                "widget_type": "text",
                "config": {
                  "text": {
                    "en": "123 Main Street, City, Country",
                    "pl": "ul. Glowna 123, Miasto, Polska"
                  }
                },
                "settings": {}
              }
            ]
          }
        ]
      }
    ],
    "content_flat": {
      "email": "hello@example.com",
      "phone": "+1 234 567 890",
      "address": "123 Main Street, City, Country"
    },
    "metadata": {
      "code": "contact-info",
      "element_uuid": "650e8400-e29b-41d4-a716-446655440000",
      "is_public": true,
      "url": null,
      "created_at": "2024-01-15T10:00:00Z",
      "updated_at": "2024-12-15T14:30:00Z"
    }
  }
}
```

### Response Structure

#### `content`

Array of page builder sections (same structure as pages). Each section contains columns with transformed widgets. Widgets in the single-element response are transformed to the unified API format with `widget_type`, `config`, and `settings`.

| Field | Type | Description |
|-------|------|-------------|
| `uuid` | string | Unique section identifier |
| `columns_count` | number | Number of columns in the section |
| `order` | number | Section display order |
| `columns` | array | Array of columns with widgets |
| `settings` | object | Section-level style settings |

#### `content_flat`

A flattened key-value map extracted from schema fields. Useful when you need simple access to field values without traversing the section/column structure.

```json
{
  "email": "hello@example.com",
  "phone": "+1 234 567 890",
  "address": "123 Main Street, City, Country"
}
```

**Note:** `content_flat` only includes fields that have a `field_code` and `data` value in the raw content. Page builder widgets (heading, image, etc.) are not included in the flat format -- use `content` for those.

#### `metadata`

| Field | Type | Description |
|-------|------|-------------|
| `code` | string | Unique element identifier |
| `element_uuid` | string | Element UUID |
| `is_public` | boolean | Whether element is published (default: `true`) |
| `url` | string\|null | Optional URL path |
| `created_at` | string | ISO 8601 creation timestamp |
| `updated_at` | string | ISO 8601 last update timestamp |

### Response Headers

```
Cache-Tag: project-{project_uuid} project-{project_uuid}-elements
```

### Error Response

If element not found:

```json
{
  "error": "Element not found",
  "message": "Element with code 'nonexistent' not found"
}
```

HTTP Status: `404 Not Found`

---

## Preview Element (Draft)

Get a preview of an element draft using a preview token.

```
GET /v1/:workspace_code/:project_code/elements/preview/:preview_token
```

### Path Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `preview_token` | string | **Required.** Preview token generated in CMS |

### Example Request

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace/project/elements/preview/def456uvw012
```

### Example Response

```json
{
  "data": {
    "sections": [
      {
        "uuid": "section-contact",
        "columns_count": 2,
        "order": 0,
        "columns": [
          {
            "uuid": "col-c1",
            "widgets": [
              {
                "uuid": "widget-c1",
                "widget_type": "icon-list",
                "config": { ... },
                "settings": {}
              }
            ]
          }
        ],
        "settings": {}
      }
    ],
    "metadata": {
      "is_public": true
    },
    "code": "contact-info",
    "element_uuid": "650e8400-e29b-41d4-a716-446655440000",
    "created_at": "2024-01-15T10:00:00Z",
    "updated_at": "2024-12-15T14:30:00Z",
    "is_draft": true
  }
}
```

**Note:** Draft previews are not cached.

---

## Elements Inside Pages

Elements can be embedded directly into page content. When a page has a section with `grid_type: "element"` and an `element_code`, the API automatically resolves it and replaces it with the actual element sections. This happens transparently -- the consuming application receives fully resolved content.

---

## Use Cases

### Fetch and Render an Element

```javascript
async function loadElement(code) {
  const response = await fetch(
    `https://api.lesscms.com/v1/workspace/project/elements/${code}`,
    {
      headers: { 'x-api-key': API_KEY }
    }
  );

  const { data } = await response.json();

  // Render page builder sections (same as page rendering)
  for (const section of data.content || []) {
    for (const column of section.columns || []) {
      for (const widget of column.widgets || []) {
        renderWidget(widget.widget_type, widget.config);
      }
    }
  }
}
```

### Quick Access with content_flat

```javascript
// When you just need simple field values
const response = await fetch(
  'https://api.lesscms.com/v1/workspace/project/elements/contact-info',
  {
    headers: { 'x-api-key': API_KEY }
  }
);

const { data } = await response.json();

// Easy access without traversing sections
const email = data.content_flat.email;
const phone = data.content_flat.phone;

document.querySelector('.footer-email').textContent = email;
document.querySelector('.footer-phone').textContent = phone;
```

### Load All Elements

```javascript
async function loadAllElements() {
  const response = await fetch(
    'https://api.lesscms.com/v1/workspace/project/elements',
    {
      headers: { 'x-api-key': API_KEY }
    }
  );

  const { data: elements } = await response.json();

  // Create a map for quick access by code
  const elementMap = {};
  elements.forEach(el => {
    elementMap[el.code] = el;
  });

  return elementMap;
}
```

---

## Related Resources

- [Pages](pages.md)
- [Blocks](blocks.md)
- [Config](config.md)
