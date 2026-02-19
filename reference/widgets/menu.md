# Menu Widget

A navigation menu widget that displays a menu defined in LessCMS with logo, CTA button, and responsive hamburger support.

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
| `config.logo_light` | string\|null | Light logo image URL |
| `config.logo_dark` | string\|null | Dark logo image URL |
| `config.logo_height` | number | Logo height in pixels |
| `config.logo_position` | string | Logo position: `"left"`, `"center"`, `"right"` (default: `"left"`) |
| `config.layout` | string | Menu layout: `"horizontal"`, `"vertical"`, `"centered"` (default: `"horizontal"`) |
| `config.hamburger_breakpoint` | string | When to show hamburger: `"never"`, `"mobile"`, `"tablet"` (default: `"never"`) |
| `config.items_gap` | string | Gap between menu items: `"sm"`, `"md"`, `"lg"` (default: `"md"`) |
| `config.link_color` | string | Menu link color (hex) |
| `config.link_hover_color` | string | Menu link hover color (hex) |
| `config.cta_text` | string | CTA button text |
| `config.cta_position` | string | CTA position: `"left"`, `"right"`, `"below"` (default: `"right"`) |
| `config.cta_link_type` | string | CTA link type: `"custom"`, `"page"`, `"entry"` |
| `config.cta_url` | string | CTA URL (when link type is custom) |
| `config.cta_page_id` | string | CTA page ID (when link type is page) |
| `config.cta_collection_code` | string | CTA collection code (when link type is entry) |
| `config.cta_entry_id` | string | CTA entry ID (when link type is entry) |
| `config.cta_route_uuid` | string | CTA route UUID |
| `config.cta_target_blank` | boolean | Open CTA in new tab |
| `config.cta_style` | string | CTA button style (Bootstrap variants) |
| `config.cta_size` | string | CTA button size: `"sm"`, `"md"`, `"lg"` |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "menu",
  "uuid": "menu-123",
  "config": {
    "menu_code": "main-nav",
    "logo_light": "https://cdn.example.com/logo.png",
    "logo_dark": "https://cdn.example.com/logo-dark.png",
    "logo_height": 40,
    "logo_position": "left",
    "layout": "horizontal",
    "hamburger_breakpoint": "mobile",
    "items_gap": "md",
    "link_color": "#333333",
    "link_hover_color": "#50a5f1",
    "cta_text": "Get Started",
    "cta_position": "right",
    "cta_link_type": "custom",
    "cta_url": "/signup",
    "cta_page_id": "",
    "cta_collection_code": "",
    "cta_entry_id": "",
    "cta_route_uuid": "",
    "cta_target_blank": false,
    "cta_style": "primary",
    "cta_size": "md"
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
        "pl": "Strona glowna"
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
| `centered` | Logo centered with items on both sides |

## Hamburger Breakpoint Values

| Value | Description |
|-------|-------------|
| `never` | Always show full menu |
| `mobile` | Show hamburger on mobile devices |
| `tablet` | Show hamburger on tablet and smaller |

## Items Gap Values

| Value | Description |
|-------|-------------|
| `sm` | Small gap between items |
| `md` | Medium gap between items |
| `lg` | Large gap between items |

## CTA Button Styles

| Value | Description |
|-------|-------------|
| `primary` | Primary button |
| `secondary` | Secondary button |
| `success` | Success (green) button |
| `danger` | Danger (red) button |
| `warning` | Warning (yellow) button |
| `info` | Info (blue) button |
| `light` | Light button |
| `dark` | Dark button |
| `outline-primary` | Outlined primary button |
| `outline-secondary` | Outlined secondary button |
| `outline-success` | Outlined success button |
| `outline-danger` | Outlined danger button |
| `outline-warning` | Outlined warning button |
| `outline-info` | Outlined info button |

## Usage Example

```javascript
async function renderMenu(widget, language, api) {
  const {
    menu_code, layout, logo_light, logo_height, logo_position,
    hamburger_breakpoint, items_gap, link_color, link_hover_color,
    cta_text, cta_url, cta_style, cta_size
  } = widget.config;

  const menu = await api.getMenu(menu_code);
  if (!menu?.items) return '';

  const items = renderMenuItems(menu.items, language);

  const logoHtml = logo_light ? `
    <div class="menu-logo">
      <img src="${logo_light}" alt="Logo" style="height: ${logo_height || 40}px;">
    </div>
  ` : '';

  const ctaHtml = cta_text ? `
    <a href="${cta_url}" class="btn btn-${cta_style} btn-${cta_size}">${cta_text}</a>
  ` : '';

  return `
    <nav class="menu-widget menu-${layout}">
      ${logoHtml}
      <ul class="menu-list" style="gap: var(--items-gap-${items_gap});">${items}</ul>
      ${ctaHtml}
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
