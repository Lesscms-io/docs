# Redirects

URL redirect management.

## Endpoints

<span class="endpoint-badge get">GET</span> `/v1/:workspace_code/:project_code/redirects`

<span class="endpoint-badge get">GET</span> `/v1/:workspace_code/:project_code/redirects/:source_path`

## List All Redirects

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace/project/redirects
```

### Response

```json
{
  "redirects": [
    {
      "source_path": "/old-page",
      "target_path": "/new-page",
      "status_code": 301,
      "is_active": true
    },
    {
      "source_path": "/blog/*",
      "target_path": "/articles/*",
      "status_code": 302,
      "is_active": true
    }
  ]
}
```

## Get Redirect by Source Path

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace/project/redirects/old-page
```

### Response

```json
{
  "source_path": "/old-page",
  "target_path": "/new-page",
  "status_code": 301,
  "is_active": true
}
```

## Redirect Object

| Field | Type | Description |
|-------|------|-------------|
| `source_path` | string | Original URL path (supports wildcards) |
| `target_path` | string | Destination URL path |
| `status_code` | number | HTTP status code (`301`, `302`) |
| `is_active` | boolean | Whether redirect is active |

## Use Case

```javascript
// Implement redirects in your app
app.use(async (req, res, next) => {
  const { redirects } = await fetchRedirects();

  const redirect = redirects.find(r =>
    r.is_active && matchPath(req.path, r.source_path)
  );

  if (redirect) {
    return res.redirect(redirect.status_code, redirect.target_path);
  }

  next();
});
```

## Wildcard Matching

Redirects support wildcards (`*`) for dynamic matching:

```
/blog/* → /articles/*
```

Matches:
- `/blog/hello` → `/articles/hello`
- `/blog/world` → `/articles/world`
