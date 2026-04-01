# Category Grid Widget

> **LessCommerce Widget** — This widget requires an active [LessCommerce](https://lesscommerce.io) account connected to your LessCMS project.

Displays product categories in a responsive grid layout. Each category card links to the corresponding category page in the store.

## Widget Type

```
category-grid
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"category-grid"` |
| `uuid` | string | Unique widget identifier |
| `widget.config` | object | Config element group |
| `widget.config.columns` | number | Number of columns on desktop |
| `widget.config.columns_mobile` | number | Number of columns on mobile |
| `widget.config.show_description` | boolean | Whether to display category descriptions |
| `widget.heading` | object | Heading element group |
| `widget.heading.text` | object | Multilingual heading text |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "category-grid",
  "uuid": "cg-202",
  "widget": {
    "config": {
      "columns": 3,
      "columns_mobile": 1,
      "show_description": false
    },
    "heading": {
      "text": {
        "en": "Shop by Category",
        "pl": "Kategorie"
      }
    }
  },
  "settings": {
    "padding_top": 20,
    "padding_bottom": 20,
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Usage Example

```javascript
function renderCategoryGrid(widget, categories, language) {
  const { config, heading } = widget.widget;
  const title = heading?.text?.[language] || heading?.text?.en || '';

  let html = '';
  if (title) {
    html += `<h2>${title}</h2>`;
  }

  html += `<div class="category-grid" style="display: grid; grid-template-columns: repeat(${config.columns}, 1fr); gap: 20px;">`;

  for (const category of categories) {
    html += `<a href="/category/${category.slug}" class="category-card">`;
    if (category.image) {
      html += `<img src="${category.image}" alt="${category.name}" />`;
    }
    html += `<h3>${category.name}</h3>`;
    if (config.show_description && category.description) {
      html += `<p>${category.description}</p>`;
    }
    html += `</a>`;
  }

  html += `</div>`;
  return html;
}
```
