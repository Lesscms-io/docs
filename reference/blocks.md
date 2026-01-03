# Blocks

Reusable content components that can be used across pages. Perfect for headers, footers, sidebars, and other shared content.

## Endpoints

<span class="endpoint-badge get">GET</span> `/v1/:workspace_code/:project_code/blocks`

<span class="endpoint-badge get">GET</span> `/v1/:workspace_code/:project_code/blocks/:code`

<span class="endpoint-badge get">GET</span> `/v1/:workspace_code/:project_code/blocks/preview/:preview_token`

---

## List All Blocks

Get all public blocks in a project.

```
GET /v1/:workspace_code/:project_code/blocks
```

### Example Request

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace/project/blocks
```

### Example Response

```json
{
  "data": [
    {
      "block_uuid": "550e8400-e29b-41d4-a716-446655440000",
      "code": "header",
      "name": "Site Header",
      "name_translation": {
        "en": "Site Header",
        "pl": "Nagłówek strony"
      },
      "description": "Main site header with navigation",
      "description_translation": {
        "en": "Main site header with navigation",
        "pl": "Główny nagłówek strony z nawigacją"
      },
      "schema_name": "Header Template",
      "schema_code": "header-template",
      "sort_order": 1,
      "created_at": "2024-01-15T10:00:00Z",
      "updated_at": "2024-12-15T14:30:00Z",
      "data": {
        "logo_url": "https://cdn.example.com/logo.png",
        "tagline": {
          "en": "Build amazing websites",
          "pl": "Twórz niesamowite strony"
        }
      },
      "metadata": {
        "is_public": true
      }
    },
    {
      "block_uuid": "660e8400-e29b-41d4-a716-446655440001",
      "code": "footer",
      "name": "Site Footer",
      "name_translation": {
        "en": "Site Footer",
        "pl": "Stopka strony"
      },
      "sort_order": 2,
      "created_at": "2024-01-15T10:00:00Z",
      "updated_at": "2024-12-10T09:15:00Z",
      "data": {
        "copyright": "© 2024 Company Name",
        "social_links": [
          { "platform": "twitter", "url": "https://twitter.com/company" },
          { "platform": "linkedin", "url": "https://linkedin.com/company" }
        ]
      },
      "metadata": {
        "is_public": true
      }
    }
  ]
}
```

### Response Headers

```
Cache-Tag: project-{project_uuid} project-{project_uuid}-blocks
```

---

## Get Single Block

Retrieve a specific block by its code.

```
GET /v1/:workspace_code/:project_code/blocks/:code
```

### Path Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `code` | string | **Required.** Block code (e.g., `header`, `footer`) |

### Example Request

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace/project/blocks/header
```

### Example Response

```json
{
  "data": {
    "content": {
      "logo_url": "https://cdn.example.com/logo.png",
      "tagline": {
        "en": "Build amazing websites",
        "pl": "Twórz niesamowite strony"
      },
      "nav_items": [
        {
          "label": { "en": "Home", "pl": "Strona główna" },
          "url": "/"
        },
        {
          "label": { "en": "About", "pl": "O nas" },
          "url": "/about"
        }
      ]
    },
    "metadata": {
      "code": "header",
      "block_uuid": "550e8400-e29b-41d4-a716-446655440000",
      "created_at": "2024-01-15T10:00:00Z",
      "updated_at": "2024-12-15T14:30:00Z",
      "is_public": true
    }
  }
}
```

### Response Structure

#### `content`

Contains all block field data as defined in the block schema. Multilingual fields have language-keyed objects.

#### `metadata`

| Field | Type | Description |
|-------|------|-------------|
| `code` | string | Unique block identifier |
| `block_uuid` | string | Block UUID |
| `created_at` | string | ISO 8601 creation timestamp |
| `updated_at` | string | ISO 8601 last update timestamp |
| `is_public` | boolean | Whether block is published |

### Response Headers

```
Cache-Tag: project-{project_uuid} project-{project_uuid}-blocks
```

### Error Response

If block not found:

```json
{
  "error": "Block not found",
  "message": "Block with code 'nonexistent' not found"
}
```

HTTP Status: `404 Not Found`

---

## Preview Block (Draft)

Get a preview of a block draft using a preview token.

```
GET /v1/:workspace_code/:project_code/blocks/preview/:preview_token
```

### Path Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `preview_token` | string | **Required.** Preview token generated in CMS |

### Example Request

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace/project/blocks/preview/abc123xyz789
```

### Example Response

```json
{
  "data": {
    "logo_url": "https://cdn.example.com/logo-new.png",
    "tagline": {
      "en": "Updated tagline",
      "pl": "Zaktualizowany slogan"
    },
    "metadata": {
      "is_public": true
    },
    "code": "header",
    "block_uuid": "550e8400-e29b-41d4-a716-446655440000",
    "created_at": "2024-01-15T10:00:00Z",
    "updated_at": "2024-12-15T14:30:00Z",
    "is_draft": true
  }
}
```

**Note:** Draft previews are not cached.

---

## Use Cases

### Load Header Block

```javascript
async function loadHeader(language = 'en') {
  const response = await fetch(
    'https://api.lesscms.com/v1/workspace/project/blocks/header',
    {
      headers: { 'x-api-key': API_KEY }
    }
  );

  const { data } = await response.json();

  return {
    logoUrl: data.content.logo_url,
    tagline: data.content.tagline[language],
    navItems: data.content.nav_items?.map(item => ({
      label: item.label[language],
      url: item.url
    }))
  };
}
```

### Load Footer Block

```javascript
async function loadFooter() {
  const response = await fetch(
    'https://api.lesscms.com/v1/workspace/project/blocks/footer',
    {
      headers: { 'x-api-key': API_KEY }
    }
  );

  const { data } = await response.json();

  return {
    copyright: data.content.copyright,
    socialLinks: data.content.social_links
  };
}
```

### Cache Blocks Globally

```javascript
// Blocks rarely change - cache aggressively
const blockCache = new Map();

async function getBlock(code) {
  if (blockCache.has(code)) {
    return blockCache.get(code);
  }

  const response = await fetch(
    `https://api.lesscms.com/v1/workspace/project/blocks/${code}`,
    {
      headers: { 'x-api-key': API_KEY }
    }
  );

  const { data } = await response.json();
  blockCache.set(code, data);

  return data;
}
```

### Load All Blocks at Once

```javascript
async function loadAllBlocks() {
  const response = await fetch(
    'https://api.lesscms.com/v1/workspace/project/blocks',
    {
      headers: { 'x-api-key': API_KEY }
    }
  );

  const { data: blocks } = await response.json();

  // Create a map for quick access
  const blockMap = {};
  blocks.forEach(block => {
    blockMap[block.code] = block.data;
  });

  return blockMap;
}

// Usage
const blocks = await loadAllBlocks();
const headerLogo = blocks.header?.logo_url;
const footerCopyright = blocks.footer?.copyright;
```

---

## Related Resources

- [Pages](pages.md)
- [Elements](elements.md)
- [Menus](menus.md)
