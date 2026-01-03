# Caching Strategy

Optimize performance and reduce API calls with effective caching.

## Cache-Tag Headers

The API includes `Cache-Tag` headers for efficient cache invalidation:

```
Cache-Tag: project-{uuid} page-{code}
```

### Example

```bash
curl -I -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace/project/pages/homepage
```

Response headers:

```
HTTP/1.1 200 OK
Cache-Tag: project-660e8400-e29b project-660e8400-e29b-pages page-homepage
X-Workspace: workspace123
X-Project: project456
```

## CDN Caching

### Cloudflare Example

Use Cache-Tag headers with Cloudflare:

```javascript
const response = await fetch(apiUrl, {
  headers: { 'x-api-key': API_KEY }
});

const cacheTag = response.headers.get('Cache-Tag');

// Cache in Cloudflare with tag
const cfResponse = new Response(response.body, {
  headers: {
    'Cache-Control': 'public, max-age=3600',
    'Cache-Tag': cacheTag
  }
});
```

Invalidate by tag when content changes:

```bash
curl -X POST "https://api.cloudflare.com/client/v4/zones/{zone_id}/purge_cache" \
  -H "Authorization: Bearer {api_token}" \
  -H "Content-Type: application/json" \
  --data '{"tags":["page-homepage"]}'
```

## Application-Level Caching

### In-Memory Cache (Node.js)

```javascript
const NodeCache = require('node-cache');
const cache = new NodeCache({ stdTTL: 3600 }); // 1 hour

async function getPage(code) {
  const cacheKey = `page:${code}`;

  // Check cache first
  const cached = cache.get(cacheKey);
  if (cached) {
    return cached;
  }

  // Fetch from API
  const response = await fetch(`${baseUrl}/pages/${code}`, {
    headers: { 'x-api-key': API_KEY }
  });

  const page = await response.json();

  // Store in cache
  cache.set(cacheKey, page);

  return page;
}
```

### Redis Cache

```javascript
const redis = require('redis');
const client = redis.createClient();

async function getPageCached(code) {
  const cacheKey = `page:${code}`;

  // Try cache first
  const cached = await client.get(cacheKey);
  if (cached) {
    return JSON.parse(cached);
  }

  // Fetch from API
  const page = await fetchPage(code);

  // Cache for 1 hour
  await client.setex(cacheKey, 3600, JSON.stringify(page));

  return page;
}
```

## Static Site Generation

For static sites, fetch content during build time:

```javascript
// Next.js example
export async function getStaticProps() {
  const pages = await fetch(`${baseUrl}/pages`, {
    headers: { 'x-api-key': process.env.LESSCMS_API_KEY }
  }).then(r => r.json());

  return {
    props: { pages },
    revalidate: 3600 // Revalidate every hour
  };
}
```

## Best Practices

### 1. Set Appropriate TTLs

Choose TTL based on content update frequency:

- **Rarely changes** (e.g., About page): 1 hour - 24 hours
- **Occasionally changes** (e.g., Homepage): 5-15 minutes
- **Frequently changes** (e.g., Blog feed): 1-5 minutes

### 2. Cache at Multiple Levels

Layer caches for maximum performance:

```
Browser Cache (5 min)
  ↓
CDN Cache (15 min)
  ↓
Application Cache (1 hour)
  ↓
API
```

### 3. Use Stale-While-Revalidate

Serve stale content while fetching fresh data:

```javascript
async function getWithSWR(url) {
  const cached = cache.get(url);

  // Return cached immediately
  if (cached) {
    // Refresh in background
    fetchAndCache(url);
    return cached;
  }

  // No cache, fetch now
  return await fetchAndCache(url);
}
```

### 4. Invalidate on Webhooks

Set up webhooks to invalidate cache when content changes:

```javascript
app.post('/webhook/content-updated', (req, res) => {
  const { resource, code } = req.body;

  // Clear specific cache
  cache.del(`${resource}:${code}`);

  res.sendStatus(200);
});
```

## Cache Invalidation Strategies

### 1. Time-Based (TTL)

Simple but may serve stale content:

```javascript
cache.set(key, value, { ttl: 3600 }); // Expire after 1 hour
```

### 2. Event-Based (Webhooks)

Invalidate immediately when content changes.

### 3. Manual Purge

Provide admin UI to manually purge cache.

### 4. Smart Invalidation

Use Cache-Tag headers to invalidate related content:

```javascript
function invalidateTags(tags) {
  tags.forEach(tag => {
    // Find all cache entries with this tag
    const keys = cache.keys().filter(key =>
      cache.get(key)?._tags?.includes(tag)
    );

    // Delete them
    keys.forEach(key => cache.del(key));
  });
}
```

## Related

- [Best Practices](best-practices.md)
- [Error Handling](errors.md)
