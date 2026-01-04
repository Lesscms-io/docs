# Menu Widget

A navigation menu widget that displays a menu defined in LessCMS.

## Widget Type

```
menu
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"menu"` |
| `uuid` | string | Unique widget identifier |
| `config` | object | Widget configuration |
| `config.menu_code` | string | Code of the menu to display |
| `config.layout` | string | Menu layout: `"horizontal"` or `"vertical"` |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "menu",
  "uuid": "menu-123",
  "config": {
    "menu_code": "main-nav",
    "layout": "horizontal"
  },
  "settings": {
    "paddingTop": 16,
    "paddingBottom": 16,
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Fetching Menu Data

The menu widget only contains the menu reference. To get actual menu items, fetch from the Menu API:

```
GET /api/{project}/menus/{menu_code}
```

### Menu Response Structure

```json
{
  "code": "main-nav",
  "name": "Main Navigation",
  "items": [
    {
      "id": "item-1",
      "label": {
        "en": "Home",
        "pl": "Strona główna"
      },
      "url": "/",
      "target": "_self",
      "children": []
    },
    {
      "id": "item-2",
      "label": {
        "en": "Products",
        "pl": "Produkty"
      },
      "url": "/products",
      "target": "_self",
      "children": [
        {
          "id": "item-2-1",
          "label": {
            "en": "Category A",
            "pl": "Kategoria A"
          },
          "url": "/products/category-a",
          "target": "_self"
        }
      ]
    }
  ]
}
```

## Layout Values

| Value | Description |
|-------|-------------|
| `horizontal` | Items displayed in a row (header navigation) |
| `vertical` | Items stacked vertically (sidebar navigation) |

## Usage Example

```javascript
async function renderMenu(widget, language, api) {
  const { menu_code, layout } = widget.config;

  const menu = await api.getMenu(menu_code);
  if (!menu?.items) return '';

  const items = renderMenuItems(menu.items, language);

  return `
    <nav class="menu-widget menu-${layout}">
      <ul class="menu-list">${items}</ul>
    </nav>
  `;
}

function renderMenuItems(items, language) {
  return items.map(item => {
    const label = item.label?.[language] || item.label?.en || '';
    const hasChildren = item.children?.length > 0;

    return `
      <li class="menu-item ${hasChildren ? 'has-children' : ''}">
        <a href="${item.url}" ${item.target === '_blank' ? 'target="_blank"' : ''}>${label}</a>
        ${hasChildren ? `<ul class="menu-submenu">${renderMenuItems(item.children, language)}</ul>` : ''}
      </li>
    `;
  }).join('');
}
```
