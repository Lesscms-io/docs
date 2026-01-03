# Routes

Dynamic routing configuration for your website. Returns all pages and collection URL patterns for building client-side routing.

## Endpoint

<span class="endpoint-badge get">GET</span> `/v1/:workspace_code/:project_code/routes`

## Get All Routes

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace/project/routes
```

### Response

```json
{
  "data": {
    "homepage": {
      "code": "homepage",
      "url": "/",
      "page_uuid": "550e8400-e29b-41d4-a716-446655440000",
      "name": "Homepage",
      "name_translation": {
        "en": "Homepage",
        "pl": "Strona główna"
      }
    },
    "pages": [
      {
        "code": "homepage",
        "url": "/",
        "pattern": null,
        "name": "Homepage",
        "name_translation": {
          "en": "Homepage",
          "pl": "Strona główna"
        }
      },
      {
        "code": "about",
        "url": "/about",
        "pattern": "/about",
        "name": "About Us",
        "name_translation": {
          "en": "About Us",
          "pl": "O nas"
        }
      },
      {
        "code": "contact",
        "url": "/contact",
        "pattern": "/contact",
        "name": "Contact",
        "name_translation": {
          "en": "Contact",
          "pl": "Kontakt"
        }
      }
    ],
    "collections": [
      {
        "code": "blog-posts",
        "name": "Blog Posts",
        "name_translation": {
          "en": "Blog Posts",
          "pl": "Wpisy blogowe"
        },
        "routes": [
          {
            "url_pattern": "/blog/{slug}",
            "filter_rules": null,
            "page_code": "blog-template",
            "name": "Blog post detail",
            "sort_order": 0
          },
          {
            "url_pattern": "/blog",
            "filter_rules": {"category": "news"},
            "page_code": "blog-list",
            "name": "News blog list",
            "sort_order": 1
          }
        ]
      },
      {
        "code": "products",
        "name": "Products",
        "name_translation": {
          "en": "Products",
          "pl": "Produkty"
        },
        "routes": [
          {
            "url_pattern": "/products/{slug}",
            "filter_rules": null,
            "page_code": "product-detail",
            "name": "Product detail page",
            "sort_order": 0
          }
        ]
      }
    ]
  }
}
```

### Response Fields

#### Root Object

| Field | Type | Description |
|-------|------|-------------|
| `homepage` | object\|null | Homepage page info, or `null` if not set |
| `pages` | array | All public pages with their URLs |
| `collections` | array | Collections with entry URL patterns (only those with patterns configured) |

#### Homepage Object

| Field | Type | Description |
|-------|------|-------------|
| `code` | string | Page code |
| `url` | string | Always `/` for homepage |
| `name` | string | Page name |
| `name_translation` | object | Multilingual page name |

#### Page Object

| Field | Type | Description |
|-------|------|-------------|
| `code` | string | Page code |
| `url` | string | Full URL path for the page |
| `pattern` | string\|null | Route pattern from MySQL (if configured) |
| `name` | string | Page name |
| `name_translation` | object | Multilingual page name |

#### Collection Object

| Field | Type | Description |
|-------|------|-------------|
| `code` | string | Collection code |
| `name` | string | Collection name |
| `name_translation` | object | Multilingual collection name |
| `routes` | array | Array of route configurations for this collection |

#### Collection Route Object

| Field | Type | Description |
|-------|------|-------------|
| `url_pattern` | string | URL pattern with placeholders (e.g., `/blog/{slug}`, `/pokoje/{entry_id}`) |
| `filter_rules` | object\|null | Filter rules for list routes (e.g., `{"category": "news"}`) |
| `page_code` | string\|null | Code of the page template to render this route |
| `name` | string | Human-readable name for this route |
| `sort_order` | number | Display order (lower numbers first) |

### Response Headers

```
Cache-Tag: project-{project_uuid} project-{project_uuid}-routes
```

---

## Collection Routes

Collections can have multiple routes configured. Each route defines a URL pattern that maps to a specific page template.

### Common Patterns

**Entry routes** - Display single collection entry:
- `/blog/{slug}` - Blog post by slug
- `/products/{slug}` - Product by slug
- `/pokoje/{entry_id}` - Room details by entry ID

**List routes** - Display filtered list of entries:
- `/blog` - All blog posts
- `/blog/category/news` - Filtered by category
- `/products/sale` - Products on sale

### Example: Multiple Routes

A collection can have multiple routes:

```json
{
  "code": "blog-posts",
  "routes": [
    {
      "url_pattern": "/blog",
      "filter_rules": null,
      "page_code": "blog-list",
      "name": "All Posts",
      "sort_order": 0
    },
    {
      "url_pattern": "/blog/news",
      "filter_rules": {"category": "news"},
      "page_code": "blog-list",
      "name": "News Posts",
      "sort_order": 1
    },
    {
      "url_pattern": "/blog/{slug}",
      "filter_rules": null,
      "page_code": "blog-detail",
      "name": "Post Details",
      "sort_order": 2
    }
  ]
}
```

This creates:
- `/blog` - All posts
- `/blog/news` - News posts only
- `/blog/my-first-post` - Single post detail

---

## URL Pattern Placeholders

Collection entry URL patterns support these placeholders:

| Placeholder | Description |
|-------------|-------------|
| `{slug}` | Entry's slug field (or `entry_url_field` if configured) |
| `{id}` or `{entry_id}` | Entry ID |
| `{lang}` | Current language code |
| `{collection_code}` | Collection code |
| `{any_field}` | Any field code from the entry |

---

## Use Cases

### Build Client-Side Router (Vue/React)

```javascript
async function setupRoutes() {
  const response = await fetch(
    'https://api.lesscms.com/v1/workspace/project/routes',
    { headers: { 'x-api-key': API_KEY } }
  );

  const { data } = await response.json();
  const routes = [];

  // Add page routes
  data.pages.forEach(page => {
    routes.push({
      path: page.url,
      component: () => import('./PageView.vue'),
      meta: {
        pageCode: page.code,
        isHomepage: data.homepage?.code === page.code
      }
    });
  });

  // Add collection routes (both entry and list routes)
  data.collections.forEach(collection => {
    collection.routes.forEach(route => {
      // Convert pattern to router format: /blog/{slug} -> /blog/:slug
      const routerPath = route.url_pattern.replace(/\{(\w+)\}/g, ':$1');

      routes.push({
        path: routerPath,
        component: route.type === 'list'
          ? () => import('./CollectionListView.vue')
          : () => import('./CollectionEntryView.vue'),
        meta: {
          collectionCode: collection.code,
          routeType: route.type,
          slugField: route.url_field,
          filterRules: route.filter_rules,
          pageCode: route.page_code
        }
      });
    });
  });

  return routes;
}
```

### Static Site Generation (Next.js)

```javascript
export async function getStaticPaths() {
  const response = await fetch(
    'https://api.lesscms.com/v1/workspace/project/routes',
    { headers: { 'x-api-key': process.env.LESSCMS_API_KEY } }
  );

  const { data } = await response.json();

  // Generate paths for all pages
  const pagePaths = data.pages.map(page => ({
    params: { slug: page.url.split('/').filter(Boolean) }
  }));

  return {
    paths: pagePaths,
    fallback: 'blocking'
  };
}
```

### Build Breadcrumbs

```javascript
function buildBreadcrumbs(currentUrl, routes) {
  const page = routes.pages.find(p => p.url === currentUrl);

  if (!page) return [];

  const breadcrumbs = [
    { label: routes.homepage?.name || 'Home', url: '/' }
  ];

  if (page.page_uuid !== routes.homepage?.page_uuid) {
    breadcrumbs.push({
      label: page.name,
      url: page.url
    });
  }

  return breadcrumbs;
}
```

### Generate Sitemap Links

```javascript
async function getSitemapUrls(baseUrl) {
  const { data } = await fetchRoutes();

  const urls = data.pages.map(page => ({
    loc: `${baseUrl}${page.url}`,
    changefreq: page.page_uuid === data.homepage?.page_uuid ? 'daily' : 'weekly'
  }));

  return urls;
}
```

---

## Related Resources

- [Pages](pages.md)
- [Collections](collections.md)
- [Config](config.md)
- [Sitemap](sitemap.md)
