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

## Step 3: Filtering Content

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

## Step 4: Working with Collections

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

## Step 5: Fetching Menus

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
