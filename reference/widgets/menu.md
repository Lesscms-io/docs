# Menu Widget

A navigation menu widget that displays a menu defined in LessCMS with logo, CTA button, and responsive hamburger support. Uses nested element-group structure.

## Widget Type

```
menu
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"menu"` |
| `uuid` | string | Unique widget identifier |
| `widget.link` | object | Menu link styling |
| `widget.link.color` | string\|null | Menu link text color |
| `widget.link.color:hover` | string\|null | Menu link text color on hover |
| `widget.link.background` | string\|null | Menu link background color |
| `widget.link.background:hover` | string\|null | Menu link background color on hover |
| `widget.link.hover_animation` | string | Hover animation effect: `"none"`, `"underline"`, `"overline"`, `"highlight"`, `"scale"`, `"bracket"` (default: `"none"`) |
| `widget.link.hover_animation_color` | string\|null | Color for the animation element (falls back to `link.color:hover`) |
| `widget.logo` | object | Logo settings |
| `widget.logo.light` | string\|null | Light logo image URL |
| `widget.logo.dark` | string\|null | Dark logo image URL |
| `widget.logo.height` | number | Logo height in pixels (default: `40`) |
| `widget.logo.position` | string | Logo position: `"left"`, `"center"`, `"right"` (default: `"left"`) |
| `widget.config` | object | Menu configuration |
| `widget.config.menu_code` | string | Code of the menu to display |
| `widget.config.label_field` | string\|null | Field code to use as label for menu items |
| `widget.config.layout` | string | Menu layout: `"horizontal"`, `"vertical"`, `"centered"` (default: `"horizontal"`) |
| `widget.config.hamburger_breakpoint` | string | When to show hamburger: `"never"`, `"mobile"`, `"tablet"` (default: `"never"`) |
| `widget.config.items_alignment` | string | Menu items alignment: `"left"`, `"center"`, `"right"` (default: `"left"`) |
| `widget.config.items_gap` | number | Gap between menu items in pixels (default: `12`) |
| `widget.config.items_padding` | number | Padding for each menu item link in pixels (default: `0`) |
| `widget.config.items_indent` | number | Left indent for menu items in pixels (default: `0`) |
| `widget.cta` | object | Call-to-action button |
| `widget.cta.html` | string | CTA button text |
| `widget.cta.position` | string | CTA position: `"left"`, `"right"`, `"below"` (default: `"right"`) |
| `widget.cta.link_type` | string | CTA link type: `"custom"`, `"page"`, `"entry"` |
| `widget.cta.url` | string\|null | CTA URL (when link type is custom, or server-resolved) |
| `widget.cta.page_id` | string\|null | CTA page ID (when link type is page) |
| `widget.cta.collection_code` | string\|null | CTA collection code (when link type is entry) |
| `widget.cta.entry_id` | string\|null | CTA entry ID (when link type is entry) |
| `widget.cta.route_uuid` | string\|null | CTA route UUID |
| `widget.cta.target_blank` | boolean | Open CTA in new tab (default: `false`) |
| `widget.cta.style` | string | CTA button style (Bootstrap variants, default: `"info"`) |
| `widget.cta.size` | string | CTA button size: `"sm"`, `"md"`, `"lg"` (default: `"md"`) |
| `widget.cta.border_radius` | string | CTA button border radius: `"sm"`, `"md"`, `"lg"`, `"pill"` (default: `"md"`) |
| `widget.cta.icon` | string\|null | CTA button icon class (e.g., `"bx bx-right-arrow-alt"`) |
| `widget.cta.icon_position` | string | CTA icon position: `"left"`, `"right"` (default: `"left"`) |
| `widget.dropdown` | object | Dropdown submenu styling |
| `widget.dropdown.background` | string\|null | Dropdown submenu background color |
| `widget.dropdown.border_radius` | string | Dropdown border radius: `"none"`, `"sm"`, `"md"`, `"lg"` (default: `"md"`) |
| `widget.dropdown.shadow` | string | Dropdown box shadow: `"none"`, `"sm"`, `"md"`, `"lg"` (default: `"lg"`) |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "menu",
  "uuid": "menu-123",
  "widget": {
    "link": {
      "color": "var:text",
      "color:hover": null,
      "background": null,
      "background:hover": null,
      "hover_animation": "underline",
      "hover_animation_color": null
    },
    "logo": {
      "light": "https://cdn.example.com/logo.png",
      "dark": "https://cdn.example.com/logo-dark.png",
      "height": 40,
      "position": "left"
    },
    "config": {
      "menu_code": "main-nav",
      "label_field": null,
      "layout": "horizontal",
      "hamburger_breakpoint": "mobile",
      "items_alignment": "left",
      "items_gap": 12,
      "items_padding": 0,
      "items_indent": 0
    },
    "cta": {
      "html": "Get Started",
      "position": "right",
      "link_type": "custom",
      "url": "/signup",
      "page_id": null,
      "collection_code": null,
      "entry_id": null,
      "route_uuid": null,
      "target_blank": false,
      "style": "info",
      "size": "md",
      "border_radius": "md",
      "icon": null,
      "icon_position": "left"
    },
    "dropdown": {
      "background": null,
      "border_radius": "md",
      "shadow": "lg"
    }
  },
  "settings": {
    "padding_top": 16,
    "padding_bottom": 16,
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

## Hover Animation Values

| Value | Description |
|-------|-------------|
| `none` | No animation (default) |
| `underline` | Line grows from left to right under the link |
| `overline` | Line grows from left to right above the link |
| `highlight` | Background color fades in on hover |
| `scale` | Slight scale up on hover |
| `bracket` | Left/right bracket borders appear on hover |

## Dropdown Settings

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `dropdown.background` | string\|null | `null` | Background color (falls back to white) |
| `dropdown.border_radius` | string | `"md"` | Border radius: `"none"` (0), `"sm"` (4px), `"md"` (8px), `"lg"` (12px) |
| `dropdown.shadow` | string | `"lg"` | Box shadow: `"none"`, `"sm"`, `"md"`, `"lg"` |

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
  const { link, logo, config, cta, dropdown } = widget.widget;

  const menu = await api.getMenu(config.menu_code);
  if (!menu?.items) return '';

  const items = renderMenuItems(menu.items, language);

  const logoHtml = logo.light ? `
    <div class="menu-logo">
      <img src="${logo.light}" alt="Logo" style="height: ${logo.height || 40}px;">
    </div>
  ` : '';

  const ctaHtml = cta.html ? `
    <a href="${cta.url}" class="btn btn-${cta.style} btn-${cta.size}">${cta.html}</a>
  ` : '';

  return `
    <nav class="menu-widget menu-${config.layout}">
      ${logoHtml}
      <ul class="menu-list" style="gap: ${config.items_gap}px;">${items}</ul>
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
