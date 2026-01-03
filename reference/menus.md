# Menus

Navigation structures for your website. Supports hierarchical menus with unlimited nesting.

## Endpoints

<span class="endpoint-badge get">GET</span> `/v1/:workspace_code/:project_code/menus`

<span class="endpoint-badge get">GET</span> `/v1/:workspace_code/:project_code/menus/:code`

<span class="endpoint-badge get">GET</span> `/v1/:workspace_code/:project_code/menus/preview/:preview_token`

---

## List All Menus

Get all public menus in a project.

```
GET /v1/:workspace_code/:project_code/menus
```

### Example Request

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace123/project456/menus
```

### Example Response

```json
{
  "data": [
    {
      "menu_uuid": "550e8400-e29b-41d4-a716-446655440000",
      "code": "main-navigation",
      "name": "Main Navigation",
      "name_translation": {
        "en": "Main Navigation",
        "pl": "Nawigacja główna"
      },
      "description": "Primary site navigation",
      "description_translation": {
        "en": "Primary site navigation",
        "pl": "Główna nawigacja strony"
      },
      "sort_order": 1,
      "created_at": "2024-01-15T10:00:00Z",
      "updated_at": "2024-12-15T14:30:00Z"
    },
    {
      "menu_uuid": "660e8400-e29b-41d4-a716-446655440001",
      "code": "footer-links",
      "name": "Footer Links",
      "name_translation": {
        "en": "Footer Links",
        "pl": "Linki w stopce"
      },
      "sort_order": 2,
      "created_at": "2024-01-15T10:00:00Z",
      "updated_at": "2024-12-10T09:15:00Z"
    }
  ]
}
```

### Response Headers

```
Cache-Tag: project-{project_uuid} project-{project_uuid}-menus
```

---

## Get Single Menu

Retrieve a specific menu by its code with all nodes.

```
GET /v1/:workspace_code/:project_code/menus/:code
```

### Path Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `code` | string | **Required.** Menu code (e.g., `main-navigation`) |

### Example Request

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace/project/menus/main-navigation
```

### Example Response

```json
{
  "data": {
    "content": [
      {
        "uuid": "node-1",
        "label": {
          "en": "Home",
          "pl": "Strona główna"
        },
        "metadata": {
          "link_type": "page",
          "link_target": "550e8400-e29b-41d4-a716-446655440000",
          "url": "/",
          "target": "_self",
          "is_public": true
        },
        "children": []
      },
      {
        "uuid": "node-2",
        "label": {
          "en": "Products",
          "pl": "Produkty"
        },
        "metadata": {
          "link_type": "collection",
          "link_target": "660e8400-e29b-41d4-a716-446655440001",
          "url": "/products",
          "target": "_self",
          "is_public": true
        },
        "children": [
          {
            "uuid": "node-2-1",
            "label": {
              "en": "Category 1",
              "pl": "Kategoria 1"
            },
            "metadata": {
              "link_type": "custom",
              "custom_route": "/products/category-1",
              "url": "/products/category-1",
              "target": "_self",
              "is_public": true
            },
            "children": []
          },
          {
            "uuid": "node-2-2",
            "label": {
              "en": "Category 2",
              "pl": "Kategoria 2"
            },
            "metadata": {
              "link_type": "custom",
              "custom_route": "/products/category-2",
              "url": "/products/category-2",
              "target": "_self",
              "is_public": true
            },
            "children": []
          }
        ]
      },
      {
        "uuid": "node-3",
        "label": {
          "en": "Contact",
          "pl": "Kontakt"
        },
        "metadata": {
          "link_type": "page",
          "link_target": "770e8400-e29b-41d4-a716-446655440002",
          "url": "/contact",
          "target": "_self",
          "is_public": true
        },
        "children": []
      }
    ],
    "metadata": {
      "code": "main-navigation",
      "menu_uuid": "550e8400-e29b-41d4-a716-446655440000",
      "created_at": "2024-01-15T10:00:00Z",
      "updated_at": "2024-12-15T14:30:00Z"
    }
  }
}
```

### Response Structure

#### `content`

Array of menu nodes (tree structure). Each node can have `children` for nested items.

#### `metadata`

| Field | Type | Description |
|-------|------|-------------|
| `code` | string | Unique menu identifier |
| `menu_uuid` | string | Menu UUID |
| `created_at` | string | ISO 8601 creation timestamp |
| `updated_at` | string | ISO 8601 last update timestamp |

### Response Headers

```
Cache-Tag: project-{project_uuid} project-{project_uuid}-menus
```

### Error Response

If menu not found:

```json
{
  "error": "Menu not found",
  "message": "Menu with code 'nonexistent' not found"
}
```

HTTP Status: `404 Not Found`

---

## Menu Node Structure

Each menu node contains:

| Field | Type | Description |
|-------|------|-------------|
| `uuid` | string | Unique node identifier |
| `label` | object | Multilingual label text |
| `metadata` | object | Node configuration and URL |
| `children` | array | Nested menu nodes |

### Node Metadata

| Field | Type | Description |
|-------|------|-------------|
| `link_type` | string | Type of link: `page`, `collection`, `entry`, or `custom` |
| `link_target` | string | UUID of linked resource (for page/collection/entry types) |
| `custom_route` | string | Custom URL (for custom link type) |
| `url` | string | **Generated URL** - resolved from link_type/link_target |
| `target` | string | Link target: `_self` or `_blank` |
| `is_public` | boolean | Whether node is visible |

### Link Types

| Type | Description |
|------|-------------|
| `page` | Links to a page (link_target = page_uuid) |
| `collection` | Links to a collection list (link_target = collection_uuid) |
| `entry` | Links to a collection entry (link_target = entry_id) |
| `custom` | Custom URL (uses custom_route) |

---

## Preview Menu (Draft)

Get a preview of a menu draft using a preview token.

```
GET /v1/:workspace_code/:project_code/menus/preview/:preview_token
```

### Path Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `preview_token` | string | **Required.** Preview token generated in CMS |

### Example Request

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace/project/menus/preview/abc123xyz789
```

### Example Response

```json
{
  "data": {
    "nodes": [ /* same structure as content */ ],
    "code": "main-navigation",
    "menu_uuid": "550e8400-e29b-41d4-a716-446655440000",
    "created_at": "2024-01-15T10:00:00Z",
    "updated_at": "2024-12-15T14:30:00Z",
    "is_draft": true
  }
}
```

**Note:** Draft previews are not cached.

---

## Use Cases

### Render Navigation Menu

```javascript
async function renderMenu(menuCode, language = 'en') {
  const response = await fetch(
    `https://api.lesscms.com/v1/workspace/project/menus/${menuCode}`,
    {
      headers: { 'x-api-key': API_KEY }
    }
  );

  const { data } = await response.json();

  return data.content.map(node => renderNode(node, language));
}

function renderNode(node, language) {
  return {
    label: node.label[language] || node.label.en,
    url: node.metadata.url,
    target: node.metadata.target,
    children: node.children.map(child => renderNode(child, language))
  };
}
```

### Build Recursive Navigation Component (Vue)

```vue
<template>
  <nav>
    <MenuNode
      v-for="node in menu"
      :key="node.uuid"
      :node="node"
      :language="language"
    />
  </nav>
</template>

<script>
// MenuNode.vue (recursive component)
export default {
  name: 'MenuNode',
  props: ['node', 'language'],
  template: `
    <li>
      <a :href="node.metadata.url" :target="node.metadata.target">
        {{ node.label[language] }}
      </a>
      <ul v-if="node.children.length">
        <MenuNode
          v-for="child in node.children"
          :key="child.uuid"
          :node="child"
          :language="language"
        />
      </ul>
    </li>
  `
}
</script>
```

### Flatten Menu for Mobile

```javascript
function flattenMenu(nodes, depth = 0) {
  const items = [];

  nodes.forEach(node => {
    items.push({
      label: node.label,
      url: node.metadata.url,
      depth: depth
    });

    if (node.children.length) {
      items.push(...flattenMenu(node.children, depth + 1));
    }
  });

  return items;
}

// Usage
const flatMenu = flattenMenu(menu.content);
```

### Get Active Menu Item

```javascript
function findActiveNode(nodes, currentPath) {
  for (const node of nodes) {
    if (node.metadata.url === currentPath) {
      return node;
    }

    if (node.children.length) {
      const found = findActiveNode(node.children, currentPath);
      if (found) return found;
    }
  }

  return null;
}

// Usage
const activeNode = findActiveNode(menu.content, window.location.pathname);
```

### Build Breadcrumbs from Menu

```javascript
function buildBreadcrumbs(nodes, targetUrl, path = []) {
  for (const node of nodes) {
    const currentPath = [...path, {
      label: node.label,
      url: node.metadata.url
    }];

    if (node.metadata.url === targetUrl) {
      return currentPath;
    }

    if (node.children.length) {
      const result = buildBreadcrumbs(node.children, targetUrl, currentPath);
      if (result) return result;
    }
  }

  return null;
}

// Usage
const breadcrumbs = buildBreadcrumbs(menu.content, '/products/category-1');
// Returns: [{label: 'Products', url: '/products'}, {label: 'Category 1', url: '/products/category-1'}]
```

---

## Related Resources

- [Pages](pages.md)
- [Collections](collections.md)
- [Blocks](blocks.md)
- [Routes](routes.md)
