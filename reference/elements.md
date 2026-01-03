# Elements

Global content elements accessible across your site.

## Endpoints

<span class="endpoint-badge get">GET</span> `/v1/:workspace_code/:project_code/elements`

<span class="endpoint-badge get">GET</span> `/v1/:workspace_code/:project_code/elements/:code`

## List All Elements

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace/project/elements
```

## Get Single Element

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace/project/elements/contact-info
```

### Response

```json
{
  "data": {
    "content": {
      "sections": [
        {
          "uuid": "section-1",
          "columns": [
            {
              "uuid": "column-1",
              "content": [
                {
                  "field_code": "email",
                  "data": "hello@example.com"
                },
                {
                  "field_code": "phone",
                  "data": "+1 234 567 890"
                }
              ]
            }
          ]
        }
      ]
    },
    "content_flat": {
      "email": "hello@example.com",
      "phone": "+1 234 567 890"
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

### Metadata Fields

The `metadata` object contains system-generated information:

| Field | Type | Description |
|-------|------|-------------|
| `code` | string | Unique element identifier |
| `element_uuid` | string | Element UUID |
| `is_public` | boolean | Whether element is published (default: `true`) |
| `url` | string | Optional URL path |
| `created_at` | string | ISO 8601 creation timestamp |
| `updated_at` | string | ISO 8601 last update timestamp |

### Content Formats

Elements return content in **two formats**:

**1. `content`** - Full structured format with sections/columns (same as pages)

**2. `content_flat`** - Flattened key-value pairs for easy access:

```json
{
  "email": "hello@example.com",
  "phone": "+1 234 567 890",
  "address": "123 Main St"
}
```

Use `content_flat` when you just need simple field values without the section structure.

## Use Case

```javascript
// Fetch contact info element
const response = await fetch(
  'https://api.lesscms.com/v1/workspace/project/elements/contact-info',
  {
    headers: { 'x-api-key': API_KEY }
  }
);

const { data } = await response.json();

// Easy access with content_flat
const email = data.content_flat.email;
const phone = data.content_flat.phone;

// Render in footer
document.querySelector('.footer-email').textContent = email;
document.querySelector('.footer-phone').textContent = phone;
```
