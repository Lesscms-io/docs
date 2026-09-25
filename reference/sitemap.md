# Sitemap

XML sitemap generation for SEO.

## Endpoint

<span class="endpoint-badge get">GET</span> `/v1/:workspace_code/:project_code/sitemap.xml`

## Get Sitemap

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.io/v1/workspace/project/sitemap.xml
```

### Response

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9" xmlns:xhtml="http://www.w3.org/1999/xhtml">
  <url>
    <loc>https://example.com/</loc>
    <lastmod>2024-12-15</lastmod>
    <changefreq>weekly</changefreq>
    <priority>0.8</priority>
    <xhtml:link rel="alternate" hreflang="x-default" href="https://example.com/" />
    <xhtml:link rel="alternate" hreflang="en" href="https://example.com/en/" />
  </url>
  <url>
    <loc>https://example.com/about</loc>
    <lastmod>2024-12-10</lastmod>
    <changefreq>weekly</changefreq>
    <priority>0.8</priority>
    <xhtml:link rel="alternate" hreflang="x-default" href="https://example.com/about" />
    <xhtml:link rel="alternate" hreflang="en" href="https://example.com/en/about" />
  </url>
  <url>
    <loc>https://example.com/blog/my-first-post</loc>
    <lastmod>2024-12-01</lastmod>
    <changefreq>weekly</changefreq>
    <priority>0.6</priority>
    <xhtml:link rel="alternate" hreflang="x-default" href="https://example.com/blog/my-first-post" />
    <xhtml:link rel="alternate" hreflang="en" href="https://example.com/en/blog/my-first-post" />
  </url>
</urlset>
```

## Features

- Automatically includes all public pages with `in_sitemap: true`
- Includes collection entries with custom URLs
- Updates `lastmod` based on content changes
- Sets priority based on page hierarchy
- Includes `hreflang` alternates for multilingual sites (Google standard)
  - `x-default` for the project's default language
  - `{lang_code}` for each non-default language (e.g., `en`, `es`, `de`)
  - One alternate per language listed in project configuration

## Multilingual Support

For projects with multiple languages configured, each `<url>` includes `<xhtml:link>` elements pointing to the same content in all available languages:

```xml
<xhtml:link rel="alternate" hreflang="x-default" href="https://example.com/blog/post" />
<xhtml:link rel="alternate" hreflang="en" href="https://example.com/en/blog/post" />
<xhtml:link rel="alternate" hreflang="es" href="https://example.com/es/blog/post" />
```

This helps search engines understand that these URLs contain equivalent content in different languages.

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
