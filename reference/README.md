# API Reference

Complete reference documentation for all LessCMS Public API endpoints.

## Available Resources

### Content Resources

- **[Pages](pages.md)** - Website pages with dynamic content
- **[Collections](collections.md)** - Structured content entries (blog posts, products, etc.)
- **[Blocks](blocks.md)** - Reusable content components
- **[Elements](elements.md)** - Global content elements

### Navigation & Structure

- **[Menus](menus.md)** - Navigation structures
- **[Routes](routes.md)** - Dynamic routing configuration

### SEO & Configuration

- **[Sitemap](sitemap.md)** - XML sitemap generation
- **[Robots.txt](robots-txt.md)** - Robots.txt configuration
- **[Redirects](redirects.md)** - URL redirects management
- **[Config](config.md)** - Project configuration

## Common Patterns

### URL Structure

All endpoints follow this pattern:

```
https://api.lesscms.com/v1/:workspace_code/:project_code/:resource
```

**Parameters:**
- `workspace_code` - Your workspace identifier
- `project_code` - Your project identifier
- `resource` - The resource type (pages, collections, menus, etc.)

### Authentication

All requests require the `x-api-key` header:

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace/project/pages
```

### Language Parameter

Most endpoints support the `language` query parameter to return content in a specific language:

```bash
# Get English content
?language=en

# Get Polish content
?language=pl
```

When specified, multilingual fields return only the requested language. Without this parameter, all languages are returned.

### Preview Mode

Most resources support preview mode via a preview token:

```
GET /:resource/preview/:preview_token
```

This allows viewing draft/unpublished content before it goes live.

### Common Query Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `language` | string | Filter content by language (e.g., `en`, `pl`) |
| `limit` | integer | Limit number of results (collections only) |
| `offset` | integer | Skip number of results (collections only) |
| `sort` | string | Sort field (prefix with `-` for descending) |
| `filter[field]` | string | Filter by field value (collections only) |
| `group_by` | string | Group results by field (collections only) |

### Response Format

All successful responses return JSON with HTTP 200 status:

```json
{
  "resource_name": [
    { /* resource data */ }
  ]
}
```

### Error Responses

Error responses include a descriptive message:

```json
{
  "error": "Resource not found"
}
```

Common HTTP status codes:
- `200` - Success
- `400` - Bad Request (invalid parameters)
- `401` - Unauthorized (invalid or missing API key)
- `404` - Not Found (resource doesn't exist)
- `500` - Internal Server Error

[Learn more about error handling →](../guides/errors.md)

### Cache Headers

Responses include cache-related headers:

```
Cache-Tag: page:homepage, project:my-project
X-Workspace: workspace123
X-Project: project456
```

Use these for efficient CDN caching and invalidation.

[Learn more about caching →](../guides/caching.md)

## Getting Started

- [Quick Start Guide](../getting-started.md)
- [Authentication](../guides/authentication.md)
- [Best Practices](../guides/best-practices.md)
