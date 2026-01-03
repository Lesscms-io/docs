# Menu Widget

A navigation menu widget that displays a menu defined in LessCMS.

## Widget Type

```
menu
```

## Data Properties

| Property | Type | Global | Description |
|----------|------|--------|-------------|
| `menu_code` | string | Yes | Code of the menu to display |
| `layout` | string | Yes | Menu layout: `horizontal` or `vertical` |

## Example Response

```json
{
  "widget_type": "menu",
  "uuid": "menu-123",
  "data": {
    "menu_code": "main-nav",
    "layout": "horizontal"
  },
  "settings": {
    "paddingTop": 16,
    "paddingBottom": 16
  }
}
```

## Menu Data

The menu widget references a menu by its code. To get the actual menu items, you need to fetch the menu from the API:

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
        },
        {
          "id": "item-2-2",
          "label": {
            "en": "Category B",
            "pl": "Kategoria B"
          },
          "url": "/products/category-b",
          "target": "_self"
        }
      ]
    },
    {
      "id": "item-3",
      "label": {
        "en": "Contact",
        "pl": "Kontakt"
      },
      "url": "/contact",
      "target": "_self",
      "children": []
    }
  ]
}
```

## Layout Values

| Value | Description |
|-------|-------------|
| `horizontal` | Items displayed in a row (typical header navigation) |
| `vertical` | Items stacked vertically (sidebar navigation) |

## Usage Example

```javascript
// Render menu widget
async function renderMenu(widget, language, api) {
  const { menu_code, layout } = widget.data;

  // Fetch menu data from API
  const menu = await api.getMenu(menu_code);

  if (!menu || !menu.items) {
    return '';
  }

  const menuItems = renderMenuItems(menu.items, language);

  return `
    <nav class="menu-widget menu-${layout}">
      <ul class="menu-list">
        ${menuItems}
      </ul>
    </nav>
  `;
}

function renderMenuItems(items, language, level = 0) {
  return items.map(item => {
    const label = item.label?.[language] || item.label?.en || '';
    const hasChildren = item.children && item.children.length > 0;

    return `
      <li class="menu-item ${hasChildren ? 'has-children' : ''}">
        <a href="${item.url}" ${item.target === '_blank' ? 'target="_blank" rel="noopener"' : ''}>
          ${label}
          ${hasChildren ? '<i class="fa-solid fa-chevron-down"></i>' : ''}
        </a>
        ${hasChildren ? `
          <ul class="menu-submenu">
            ${renderMenuItems(item.children, language, level + 1)}
          </ul>
        ` : ''}
      </li>
    `;
  }).join('');
}
```

## CSS Example (Horizontal)

```css
.menu-horizontal .menu-list {
  display: flex;
  gap: 24px;
  list-style: none;
  padding: 0;
  margin: 0;
}

.menu-horizontal .menu-item {
  position: relative;
}

.menu-horizontal .menu-item a {
  display: flex;
  align-items: center;
  gap: 6px;
  padding: 8px 0;
  color: #333;
  text-decoration: none;
}

.menu-horizontal .menu-item a:hover {
  color: #007BFF;
}

.menu-horizontal .menu-submenu {
  display: none;
  position: absolute;
  top: 100%;
  left: 0;
  min-width: 200px;
  background: white;
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
  border-radius: 8px;
  padding: 8px 0;
  list-style: none;
}

.menu-horizontal .menu-item:hover > .menu-submenu {
  display: block;
}

.menu-horizontal .menu-submenu a {
  padding: 8px 16px;
}
```

## CSS Example (Vertical)

```css
.menu-vertical .menu-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.menu-vertical .menu-item a {
  display: block;
  padding: 12px 16px;
  color: #333;
  text-decoration: none;
  border-left: 3px solid transparent;
}

.menu-vertical .menu-item a:hover,
.menu-vertical .menu-item a.active {
  background: #F8F9FA;
  border-left-color: #007BFF;
  color: #007BFF;
}

.menu-vertical .menu-submenu {
  padding-left: 16px;
}
```
