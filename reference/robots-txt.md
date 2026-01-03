# Robots.txt

Robots.txt file configuration for search engine crawlers.

## Endpoint

<span class="endpoint-badge get">GET</span> `/v1/:workspace_code/:project_code/robots.txt`

## Get Robots.txt

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace/project/robots.txt
```

### Response

```
User-agent: *
Disallow: /admin
Disallow: /private

Sitemap: https://example.com/sitemap.xml
```

## Use Case

Proxy robots.txt to your domain:

```javascript
app.get('/robots.txt', async (req, res) => {
  const response = await fetch(
    `${LESSCMS_API_URL}/robots.txt`,
    { headers: { 'x-api-key': API_KEY } }
  );

  res.set('Content-Type', 'text/plain');
  res.send(await response.text());
});
```
