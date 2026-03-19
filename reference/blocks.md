# Blocks

Simple reusable content values — a single piece of data like a phone number, logo URL, email address, or short text. Blocks are perfect for small, shared values that appear across multiple pages (e.g., in headers, footers, or sidebars).

Unlike pages and elements which use the page builder (sections/columns/widgets), blocks store just **one value** per block.

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
      "code": "logo-light",
      "name": "Logo (light)",
      "name_translation": {
        "en": "Logo (light)",
        "pl": "Logo (jasne)"
      },
      "sort_order": 1,
      "created_at": "2024-01-15T10:00:00Z",
      "updated_at": "2024-12-15T14:30:00Z",
      "data": "https://cdn.lesscms.io/projects/abc123/logo-white.png",
      "metadata": {
        "is_public": true
      }
    },
    {
      "block_uuid": "660e8400-e29b-41d4-a716-446655440001",
      "code": "main-phone",
      "name": "Main Phone Number",
      "name_translation": {
        "en": "Main Phone Number",
        "pl": "Główny numer telefonu"
      },
      "sort_order": 2,
      "created_at": "2024-01-15T10:00:00Z",
      "updated_at": "2024-12-10T09:15:00Z",
      "data": "+48 33 874 83 15",
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
| `code` | string | **Required.** Block code (e.g., `logo-light`, `main-phone`) |

### Example Request

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace/project/blocks/main-phone
```

### Example Response

```json
{
  "data": {
    "content": "+48 33 874 83 15",
    "metadata": {
      "code": "main-phone",
      "block_uuid": "660e8400-e29b-41d4-a716-446655440001",
      "created_at": "2024-01-15T10:00:00Z",
      "updated_at": "2024-12-10T09:15:00Z",
      "is_public": true
    }
  }
}
```

### Response Structure

#### `content`

The block value — a string (text, URL, phone number, etc.) or a multilingual object:

```json
// Simple string
"content": "+48 33 874 83 15"

// Multilingual
"content": {
  "en": "Contact us today",
  "pl": "Skontaktuj się z nami"
}
```

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

**Note:** Draft previews are not cached.

---

## Use Cases

### Load Block Values

```javascript
async function loadBlocks() {
  const response = await fetch(
    'https://api.lesscms.com/v1/workspace/project/blocks',
    { headers: { 'x-api-key': API_KEY } }
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
const blocks = await loadBlocks();
const logo = blocks['logo-light'];       // "https://cdn.lesscms.io/.../logo-white.png"
const phone = blocks['main-phone'];       // "+48 33 874 83 15"
```

### Use Blocks in Templates

```html
<header>
  <img src="${blocks['logo-light']}" alt="Logo" />
  <a href="tel:${blocks['main-phone']}">${blocks['main-phone']}</a>
</header>
```

---

## Related Resources

- [Pages](pages.md) - Website pages with page builder content
- [Elements](elements.md) - Reusable page builder sections (headers, footers)
- [Menus](menus.md) - Navigation structures
