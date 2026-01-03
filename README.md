# LessCMS Public API Documentation

Welcome to the LessCMS Public API documentation. This API provides programmatic access to your published content, allowing you to build websites, mobile apps, and other integrations.

## What is LessCMS API?

The LessCMS Public API is a RESTful API that allows you to retrieve published content from your LessCMS projects. It's designed for:

- **Headless CMS implementations** - Build your frontend with any technology
- **Multi-platform content delivery** - Serve content to web, mobile, and IoT devices
- **Real-time integrations** - Connect your content to external services
- **Static site generators** - Fetch content during build time

## Key Features

### 🚀 Fast & Reliable
- CDN-ready with Cache-Tag headers
- Optimized MongoDB queries
- Sub-100ms response times

### 🔒 Secure
- API key authentication
- Workspace and project-level isolation
- Public content only (no sensitive data exposure)

### 📦 Flexible Content Types
- **Pages** - Website pages with dynamic content
- **Collections** - Structured content entries (blog posts, products, etc.)
- **Menus** - Navigation structures
- **Blocks** - Reusable content components
- **Elements** - Global content elements
- **Routes** - Dynamic routing configuration

### 🌍 Multilingual
- Built-in multi-language support
- Language-specific content retrieval
- Automatic fallback handling

## Quick Example

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace123/project456/pages
```

```json
{
  "pages": [
    {
      "code": "homepage",
      "name": "Homepage",
      "is_public": true,
      "content": {
        "title": {
          "en": "Welcome to our site",
          "pl": "Witamy na naszej stronie"
        }
      }
    }
  ]
}
```

## Getting Started

Ready to start using the API? Follow these steps:

1. **[Get your API key](getting-started.md#getting-your-api-key)** - Generate an API key from your LessCMS dashboard
2. **[Make your first request](getting-started.md#your-first-request)** - Fetch content using curl or your favorite HTTP client
3. **[Explore the API](reference/)** - Browse the complete API reference

## API Versions

The LessCMS API uses URL-based versioning. The current version is **v1**.

```
https://api.lesscms.com/v1/:workspace_code/:project_code/:resource
```

We maintain backward compatibility within each major version. When breaking changes are necessary, we release a new version and support the previous version for 12 months.

[Learn more about API versioning →](guides/versioning.md)

## Base URL

All API requests should be made to:

```
https://api.lesscms.com
```

For development/testing, you can use your local instance:

```
http://localhost:3000
```

## Authentication

All API requests require authentication using an API key passed in the `x-api-key` header:

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace123/project456/pages
```

[Learn more about authentication →](guides/authentication.md)

## Rate Limiting

Currently, there are no strict rate limits, but we monitor usage to prevent abuse. Best practices:

- Cache responses when possible
- Use CDN Cache-Tag headers for efficient cache invalidation
- Batch requests when fetching multiple resources

[Learn more about best practices →](guides/best-practices.md)

## Need Help?

- 📖 **[API Reference](reference/)** - Complete endpoint documentation
- 🔧 **[Guides](guides/)** - In-depth tutorials and best practices
- 🐛 **[Report an issue](https://github.com/lesscms/api/issues)** - Found a bug? Let us know
- 💬 **[Community](https://community.lesscms.com)** - Join the discussion

## What's Next?

<div class="next-steps">

- [Getting Started →](getting-started.md)
- [Authentication Guide →](guides/authentication.md)
- [Pages API →](reference/pages.md)
- [Collections API →](reference/collections.md)

</div>

---

**Latest API Version:** v1 | **Last Updated:** December 2025
