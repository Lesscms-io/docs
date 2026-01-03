# Sitemap

XML sitemap generation for SEO.

## Endpoint

<span class="endpoint-badge get">GET</span> `/v1/:workspace_code/:project_code/sitemap.xml`

## Get Sitemap

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace/project/sitemap.xml
```

### Response

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://example.com/</loc>
    <lastmod>2024-12-15</lastmod>
    <priority>1.0</priority>
  </url>
  <url>
    <loc>https://example.com/about</loc>
    <lastmod>2024-12-10</lastmod>
    <priority>0.8</priority>
  </url>
</urlset>
```

## Features

- Automatically includes all public pages with `in_sitemap: true`
- Includes collection entries with custom URLs
- Updates `lastmod` based on content changes
- Sets priority based on page hierarchy

## Use Case

Proxy sitemap to your domain:

```javascript
app.get('/sitemap.xml', async (req, res) => {
  const response = await fetch(
    `${LESSCMS_API_URL}/sitemap.xml`,
    { headers: { 'x-api-key': API_KEY } }
  );

  res.set('Content-Type', 'application/xml');
  res.send(await response.text());
});
```
