# Category Header Widget

> **LessCommerce Widget** — This widget requires an active [LessCommerce](https://lesscommerce.io) account connected to your LessCMS project.

A category page header with breadcrumbs, category name, description, and product count. Typically placed at the top of the auto-generated system `/category/:slug` page, followed by a `product-grid` widget showing the category's products.

## Widget Type

```
category-header
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"category-header"` |
| `uuid` | string | Unique widget identifier |
| `widget.config` | object | Config element group |
| `widget.config.show_breadcrumbs` | boolean | Show breadcrumb navigation |
| `widget.config.show_description` | boolean | Show category description |
| `widget.config.show_product_count` | boolean | Show total product count |
| `widget.config.slug_source` | string | Where category slug comes from: `"url"` or `"static"` |
| `widget.config.slug_url_segment` | number | Which URL segment holds the slug (when `slug_source="url"`) |
| `widget.config.slug` | string | Hard-coded category slug (when `slug_source="static"`). Use this for landing pages dedicated to a specific category. |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "category-header",
  "uuid": "category-header-001",
  "widget": {
    "config": {
      "show_breadcrumbs": true,
      "show_description": true,
      "show_product_count": true,
      "slug_source": "url",
      "slug_url_segment": 2,
      "slug": ""
    }
  },
  "settings": {
    "padding_top": 20,
    "padding_bottom": 20,
    "responsive": { "tablet": {}, "mobile": {} }
  }
}
```

## Data Source

The widget resolves the category slug from the URL (or static config) and fetches the category from the LessCommerce API:

- `GET /api/categories/by-slug/{slug}` — fetch category details including name, description, ancestors (for breadcrumbs), and product count

## Usage Example

```javascript
async function renderCategoryHeader(widget, urlPath) {
  const { config } = widget.widget;
  const slug = urlPath.split('/').filter(Boolean)[config.slug_url_segment - 1];
  const category = await fetch(`/api/categories/by-slug/${slug}`).then(r => r.json());

  return `
    <header>
      ${config.show_breadcrumbs ? renderBreadcrumbs(category.ancestors) : ''}
      <h1>${category.name}</h1>
      ${config.show_product_count ? `<div>${category.product_count} produktów</div>` : ''}
      ${config.show_description ? `<p>${category.description}</p>` : ''}
    </header>
  `;
}
```
