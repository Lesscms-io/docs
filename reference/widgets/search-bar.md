# Search Bar Widget

> **LessCommerce Widget** — This widget requires an active [LessCommerce](https://lesscommerce.io) account connected to your LessCMS project.

A product search input for the storefront. Provides instant search functionality with configurable placeholder text and visual style.

## Widget Type

```
search-bar
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"search-bar"` |
| `uuid` | string | Unique widget identifier |
| `widget.config` | object | Config element group |
| `widget.config.placeholder` | string | Placeholder text displayed in the search input |
| `widget.config.style` | string | Visual style: `"default"`, `"rounded"`, or `"minimal"` |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "search-bar",
  "uuid": "sb-303",
  "widget": {
    "config": {
      "placeholder": "Szukaj produktów...",
      "style": "default"
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

## Search Bar Styles

| Value | Description |
|-------|-------------|
| `default` | Standard search input with border |
| `rounded` | Search input with fully rounded corners |
| `minimal` | Borderless search input with subtle underline |

## Usage Example

```javascript
function renderSearchBar(widget) {
  const { config } = widget.widget;

  const styleClass = `search-bar--${config.style}`;
  const placeholder = config.placeholder || 'Search products...';

  return `
    <form class="search-bar ${styleClass}" action="/search" method="GET">
      <input type="text" name="q" placeholder="${placeholder}" />
      <button type="submit"><i class="fa-solid fa-magnifying-glass"></i></button>
    </form>
  `;
}
```
