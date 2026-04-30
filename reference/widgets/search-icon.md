# Search Icon Widget

> **LessCommerce Widget** — This widget requires an active [LessCommerce](https://lesscommerce.io) account connected to your LessCMS project.

A compact icon-only search trigger intended for header/toolbar placement. Either opens an inline dropdown with a search input + autocomplete, or navigates to a dedicated search page URL.

## Widget Type

```
search-icon
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"search-icon"` |
| `uuid` | string | Unique widget identifier |
| `widget.config` | object | Config element group |
| `widget.config.icon` | string | Icon class (FontAwesome / Boxicons / `svg:<markup>`) (default: `"fa-solid fa-magnifying-glass"`) |
| `widget.config.size` | number | Icon size in pixels (default: `20`) |
| `widget.config.click_action` | string | Action on click: `"dropdown"` (show popover with search input) or `"navigate"` (go to URL) (default: `"dropdown"`) |
| `widget.config.navigate_url` | string | Target URL when `click_action` is `"navigate"` (default: `"/search"`) |
| `widget.config.placeholder` | string\|object\|null | Search input placeholder (multilingual) |
| `widget.icon` | object | Icon styling |
| `widget.icon.color` | string\|null | Icon color |
| `widget.icon.color:hover` | string\|null | Icon color on hover |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "search-icon",
  "uuid": "si-789",
  "widget": {
    "config": {
      "icon": "fa-solid fa-magnifying-glass",
      "size": 20,
      "click_action": "dropdown",
      "navigate_url": "/search",
      "placeholder": { "pl": "Szukaj produktów...", "en": "Search products..." }
    },
    "icon": {
      "color": "var:text",
      "color:hover": "var:primary"
    }
  },
  "settings": {
    "padding_top": 0,
    "padding_bottom": 0,
    "responsive": { "tablet": {}, "mobile": {} }
  }
}
```

## Click Actions

| Value | Description |
|-------|-------------|
| `dropdown` | Toggles an inline popover with a search input + autocomplete fed by the Storefront API |
| `navigate` | Redirects to `navigate_url` on click (no popover) |

## Usage Example

```javascript
function renderSearchIcon(widget) {
  const { config, icon } = widget.widget;
  const iconTag = `<i class="${config.icon}" style="font-size:${config.size}px"></i>`;

  if (config.click_action === 'navigate') {
    return `<a class="search-icon" href="${config.navigate_url}">${iconTag}</a>`;
  }

  return `
    <div class="search-icon">
      <button type="button" class="search-icon__trigger">${iconTag}</button>
      <div class="search-icon__popover" hidden>
        <input type="search" placeholder="${config.placeholder?.en || ''}" />
      </div>
    </div>
  `;
}
```
