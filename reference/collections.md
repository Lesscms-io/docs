# Collections

Collections are structured content types perfect for blogs, products, team members, portfolios, and any repeatable content.

## Endpoints

<span class="endpoint-badge get">GET</span> `/v1/:workspace_code/:project_code/collections`

<span class="endpoint-badge get">GET</span> `/v1/:workspace_code/:project_code/collections/:code`

<span class="endpoint-badge get">GET</span> `/v1/:workspace_code/:project_code/collections/:code/:id`

<span class="endpoint-badge get">GET</span> `/v1/:workspace_code/:project_code/collections/:code/templates/:template_id`

<span class="endpoint-badge get">GET</span> `/v1/:workspace_code/:project_code/collections/preview/:preview_token`

---

## List All Collections

Get metadata for all collections in a project.

```
GET /v1/:workspace_code/:project_code/collections
```

### Example Request

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace123/project456/collections
```

### Example Response

```json
{
  "data": [
    {
      "collection_uuid": "550e8400-e29b-41d4-a716-446655440000",
      "code": "blog-posts",
      "name": "Blog Posts",
      "name_translation": {
        "en": "Blog Posts",
        "pl": "Wpisy blogowe"
      },
      "description": "Articles and blog posts",
      "schema": { /* field definitions */ },
      "api_options": {
        "response_format": "as_is",
        "nested_by_field": null
      },
      "sort_order": 1,
      "created_at": "2024-01-15T10:00:00Z",
      "updated_at": "2024-12-15T14:30:00Z"
    }
  ]
}
```

### Response Headers

```
Cache-Tag: project-{project_uuid} project-{project_uuid}-collections
```

---

## Get Collection Entries

Retrieve all entries from a specific collection.

```
GET /v1/:workspace_code/:project_code/collections/:code
```

### Path Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `code` | string | **Required.** Collection code (e.g., `blog-posts`) |

### Query Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `page` | integer | Page number for pagination (default: `1`) |
| `pageSize` | integer | Number of entries per page (default: `20`) |
| `exclude_entry_id` | string | Exclude entry with this `entry_id` from results. Useful for "related posts" widgets that should not show the currently viewed entry. |
| `{field_name}` | string | Filter by field value (case-insensitive substring match) |

### Example Request

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace123/project456/collections/blog-posts
```

### Example Response (Flat Format)

```json
{
  "data": [
    {
      "content": {
        "title": {
          "en": "My First Blog Post",
          "pl": "Mój pierwszy wpis na blogu"
        },
        "slug": {
          "en": "my-first-blog-post",
          "pl": "moj-pierwszy-wpis"
        },
        "excerpt": {
          "en": "Introduction to our new blog",
          "pl": "Wprowadzenie do naszego nowego bloga"
        },
        "body": {
          "en": "Full article content here...",
          "pl": "Pełna treść artykułu tutaj..."
        },
        "published_date": "2024-12-01",
        "author": "John Doe",
        "category": [
          {
            "code": "technology",
            "value": "Technology",
            "value_translation": {
              "pl": "Technologia"
            }
          }
        ],
        "featured_image": {
          "url": "https://cdn.example.com/blog/image1.jpg",
          "alt": "Blog post image"
        }
      },
      "metadata": {
        "code": "my-first-post",
        "collection_uuid": "550e8400-e29b-41d4-a716-446655440000",
        "entry_id": "my-first-post",
        "created_at": "2024-12-01T10:00:00Z",
        "updated_at": "2024-12-01T10:00:00Z",
        "is_public": true,
        "url": "/blog/my-first-blog-post"
      },
      "seo": {
        "title": {
          "en": "My First Blog Post - Company Blog",
          "pl": "Mój pierwszy wpis - Blog Firmowy"
        },
        "description": {
          "en": "Introduction to our new blog",
          "pl": "Wprowadzenie do naszego nowego bloga"
        },
        "keywords": ["blog", "technology", "introduction"],
        "og_image": "https://cdn.example.com/blog/og-image1.jpg"
      }
    }
  ],
  "meta": {
    "total": 42,
    "page": 1,
    "pageSize": 20,
    "totalPages": 3,
    "format": "as_is"
  }
}
```

### Example Response (Nested Format)

When a collection is configured with `response_format: "nested"` and `nested_by_field: "category"`:

```json
{
  "data": {
    "technology": [
      {
        "content": { /* entry 1 */ },
        "metadata": { /* metadata */ }
      },
      {
        "content": { /* entry 2 */ },
        "metadata": { /* metadata */ }
      }
    ],
    "lifestyle": [
      {
        "content": { /* entry 3 */ },
        "metadata": { /* metadata */ }
      }
    ],
    "_ungrouped": [
      {
        "content": { /* entry without category */ },
        "metadata": { /* metadata */ }
      }
    ]
  },
  "meta": {
    "total": 42,
    "page": 1,
    "pageSize": 20,
    "totalPages": 3,
    "format": "nested"
  }
}
```

### Filtering Entries

Filter entries by any field using query parameters:

```bash
# Filter by author
curl -H "x-api-key: YOUR_API_KEY" \
  "https://api.lesscms.com/v1/workspace/project/collections/blog-posts?author=John"

# Filter by multiple fields
curl -H "x-api-key: YOUR_API_KEY" \
  "https://api.lesscms.com/v1/workspace/project/collections/blog-posts?author=John&category=technology"
```

Filters use case-insensitive substring matching (regex).

### Excluding Current Entry

When building "related posts" or "other entries" widgets, exclude the currently viewed entry:

```bash
# Exclude a specific entry by its entry_id
curl -H "x-api-key: YOUR_API_KEY" \
  "https://api.lesscms.com/v1/workspace/project/collections/blog-posts?exclude_entry_id=my-current-post"
```

### Pagination

```bash
# Get page 2 with 10 entries per page
curl -H "x-api-key: YOUR_API_KEY" \
  "https://api.lesscms.com/v1/workspace/project/collections/blog-posts?page=2&pageSize=10"
```

### Response Headers

```
Cache-Tag: project-{project_uuid} project-{project_uuid}-collections collection-{collection_uuid}
```

---

## Get Single Entry

Retrieve a specific entry from a collection.

```
GET /v1/:workspace_code/:project_code/collections/:code/:id
```

### Path Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `code` | string | **Required.** Collection code |
| `id` | string | **Required.** Entry ID (entry_id from metadata) |

### Example Request

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace123/project456/collections/blog-posts/my-first-post
```

### Example Response

```json
{
  "content": {
    "title": {
      "en": "My First Blog Post",
      "pl": "Mój pierwszy wpis na blogu"
    },
    "slug": {
      "en": "my-first-blog-post",
      "pl": "moj-pierwszy-wpis"
    },
    "body": {
      "en": "Full article content...",
      "pl": "Pełna treść artykułu..."
    },
    "published_date": "2024-12-01",
    "author": "John Doe"
  },
  "metadata": {
    "code": "my-first-post",
    "collection_uuid": "550e8400-e29b-41d4-a716-446655440000",
    "entry_id": "my-first-post",
    "created_at": "2024-12-01T10:00:00Z",
    "updated_at": "2024-12-01T10:00:00Z",
    "is_public": true,
    "url": "/blog/my-first-blog-post"
  },
  "seo": {
    "title": {
      "en": "My First Blog Post - Company Blog"
    },
    "description": {
      "en": "Introduction to our new blog"
    }
  }
}
```

### Response Headers

```
Cache-Tag: project-{project_uuid} collection-{collection_uuid} entry-{code}
```

### Error Response

If entry not found:

```json
{
  "error": "Entry not found",
  "message": "Entry with ID 'nonexistent-post' not found in collection 'blog-posts'"
}
```

HTTP Status: `404 Not Found`

---

## Get Entry Template

Retrieve a custom entry template definition for rendering collection entries with custom layouts.

```
GET /v1/:workspace_code/:project_code/collections/:code/templates/:template_id
```

### Path Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `code` | string | **Required.** Collection code |
| `template_id` | string | **Required.** Template UUID |

### Example Request

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace123/project456/collections/blog-posts/templates/48bd5d18-84d3-46e5-9446-d6f30a1f4123
```

### Example Response

```json
{
  "data": {
    "uuid": "48bd5d18-84d3-46e5-9446-d6f30a1f4123",
    "name": "Featured Post Card",
    "type": "template",
    "sections": [
      {
        "uuid": "section-uuid-1",
        "settings": {
          "contentWidth": "100%",
          "fullHeight": false
        },
        "columnsCount": 2,
        "columns": [
          {
            "uuid": "column-uuid-1",
            "settings": {
              "verticalAlign": "flex-start",
              "horizontalAlign": "stretch"
            },
            "content": [
              {
                "widget_type": "collection-field",
                "uuid": "widget-uuid-1",
                "config": {
                  "field_code": "featured_image",
                  "display_as": "image"
                }
              }
            ],
            "span": 6
          },
          {
            "uuid": "column-uuid-2",
            "settings": {
              "verticalAlign": "center",
              "horizontalAlign": "stretch"
            },
            "content": [
              {
                "widget_type": "collection-field",
                "uuid": "widget-uuid-2",
                "config": {
                  "field_code": "title",
                  "display_as": "h2"
                }
              },
              {
                "widget_type": "collection-field",
                "uuid": "widget-uuid-3",
                "config": {
                  "field_code": "excerpt",
                  "display_as": "p"
                }
              }
            ],
            "span": 6
          }
        ]
      }
    ]
  }
}
```

### Response Headers

```
Cache-Tag: project-{project_uuid} project-{project_uuid}-collections collection-{collection_uuid}
```

### Error Response

If template not found:

```json
{
  "error": "Template not found",
  "message": "Template with id '48bd5d18-...' not found in collection 'blog-posts'"
}
```

HTTP Status: `404 Not Found`

### Usage with Collection Widgets

When a collection widget (e.g., `collection-grid`) has `entry_template` set to `custom:{uuid}`, fetch the template definition and use it to render each entry:

```javascript
// 1. Fetch page with collection-grid widget
const page = await fetch('/v1/workspace/project/pages/home');
const { content } = await page.json();

// 2. Find collection-grid widget
const gridWidget = findWidget(content, 'collection-grid');
const { collection_code, entry_template } = gridWidget.config;

// 3. If custom template, fetch template definition
if (entry_template?.startsWith('custom:')) {
  const templateId = entry_template.replace('custom:', '');
  const templateResponse = await fetch(
    `/v1/workspace/project/collections/${collection_code}/templates/${templateId}`
  );
  const { data: template } = await templateResponse.json();

  // 4. Use template.sections to render each entry
  entries.forEach(entry => {
    renderEntryWithTemplate(entry, template.sections);
  });
}
```

---

## Preview Entry (Draft)

Get a preview of a draft entry using a preview token.

```
GET /v1/:workspace_code/:project_code/collections/preview/:preview_token
```

### Path Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `preview_token` | string | **Required.** Preview token generated in CMS |

### Example Request

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace/project/collections/preview/abc123xyz789
```

### Example Response

Same format as single entry response, but returns draft content.

---

## Data Structure

### Entry Object

Each collection entry contains three main sections:

#### `content`
Contains all custom field data as defined in the collection schema. Multilingual fields have language-keyed objects:

```json
{
  "title": {
    "en": "English title",
    "pl": "Polski tytuł"
  },
  "author": "John Doe",  // Non-multilingual field
  "published_date": "2024-12-01"
}
```

#### `metadata`
System-generated metadata about the entry:

| Field | Type | Description |
|-------|------|-------------|
| `code` | string | Unique entry identifier |
| `collection_uuid` | string | Parent collection UUID |
| `entry_id` | string | User-friendly entry ID |
| `created_at` | string | ISO 8601 creation timestamp |
| `updated_at` | string | ISO 8601 last update timestamp |
| `is_public` | boolean | Whether entry is published |
| `url` | string | Full URL path for the entry |

#### `seo` (optional)
SEO metadata if configured:

```json
{
  "title": { "en": "SEO title" },
  "description": { "en": "SEO description" },
  "keywords": ["keyword1", "keyword2"],
  "og_image": "https://cdn.example.com/image.jpg",
  "canonical_url": "https://example.com/blog/post"
}
```

### Field Types

Collections support various field types:

- **Text** - Single/multiline text
- **Rich Text** - HTML content
- **Number** - Integers or decimals
- **Date** - ISO 8601 dates
- **Boolean** - true/false
- **Select** - Single choice from options
- **Multiselect** - Multiple choices
- **Image** - Single image with URL
- **Gallery** - Array of images
- **Relation** - References to other entries
- **JSON** - Custom structured data

---

## Response Formats

Collections support two response formats configured in CMS:

### Flat Format (`as_is`)

Default format. Returns entries as a flat array:

```json
{
  "data": [ /* array of entries */ ],
  "meta": { /* pagination info */ }
}
```

### Nested Format (`nested`)

Groups entries by a specified field (useful for categories, tags, etc.):

```json
{
  "data": {
    "category-1": [ /* entries */ ],
    "category-2": [ /* entries */ ],
    "_ungrouped": [ /* entries without category */ ]
  },
  "meta": { /* pagination info */ }
}
```

The grouping field is configured in collection settings (`nested_by_field`).

---

## Use Cases

### Building a Blog

```javascript
// Fetch all blog posts
const response = await fetch(
  'https://api.lesscms.com/v1/workspace/project/collections/blog-posts?pageSize=10',
  {
    headers: { 'x-api-key': API_KEY }
  }
);

const { data: posts, meta } = await response.json();

// Render blog list
posts.forEach(post => {
  console.log(post.content.title.en);
  console.log(post.metadata.url);
});
```

### Product Catalog

```javascript
// Fetch products in specific category
const response = await fetch(
  'https://api.lesscms.com/v1/workspace/project/collections/products?category=electronics&page=1&pageSize=20',
  {
    headers: { 'x-api-key': API_KEY }
  }
);

const { data: products } = await response.json();
```

### Team Directory

```javascript
// Fetch all team members (nested by department)
const response = await fetch(
  'https://api.lesscms.com/v1/workspace/project/collections/team-members',
  {
    headers: { 'x-api-key': API_KEY }
  }
);

const { data: teamByDepartment } = await response.json();

// If nested format is enabled:
console.log(teamByDepartment.engineering); // Array of engineering team members
console.log(teamByDepartment.design);      // Array of design team members
```

---

## Best Practices

### 1. Use Pagination

Always use pagination for large collections:

```javascript
const pageSize = 20;
const page = 1;

const url = `${baseUrl}/collections/blog-posts?page=${page}&pageSize=${pageSize}`;
```

### 2. Cache Aggressively

Collections rarely change. Cache responses using Cache-Tag headers:

```javascript
const response = await fetch(url);
const cacheTag = response.headers.get('Cache-Tag');

// Store with tag for efficient invalidation
cache.set(url, data, { tags: [cacheTag], ttl: 3600 });
```

### 3. Filter Server-Side

Use query parameters to filter on the server instead of fetching all data:

```javascript
// Good
const url = `${baseUrl}/collections/posts?author=John&category=tech`;

// Bad (fetching all then filtering client-side)
const all = await fetchAll('/collections/posts');
const filtered = all.filter(p => p.content.author === 'John');
```

### 4. Handle Multilingual Content

Extract the correct language from multilingual fields:

```javascript
function getLocalizedValue(field, language = 'en') {
  if (typeof field === 'object' && !Array.isArray(field)) {
    return field[language] || field.en || Object.values(field)[0];
  }
  return field;
}

const title = getLocalizedValue(entry.content.title, 'pl');
```

---

## Related Resources

- [Pages](pages.md)
- [Blocks](blocks.md)
- [Menus](menus.md)
