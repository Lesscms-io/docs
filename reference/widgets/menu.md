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
| `widget` | object | Widget data |
| `widget.menu_code` | string | Code of the menu to display |
| `widget.label_field` | string\|null | Field code to use as label for menu items |
| `widget.logo_light` | string\|null | Light logo image URL |
| `widget.logo_dark` | string\|null | Dark logo image URL |
| `widget.logo_height` | number | Logo height in pixels |
| `widget.logo_position` | string | Logo position: `"left"`, `"center"`, `"right"` (default: `"left"`) |
| `widget.layout` | string | Menu layout: `"horizontal"`, `"vertical"`, `"centered"` (default: `"horizontal"`) |
| `widget.hamburger_breakpoint` | string | When to show hamburger: `"never"`, `"mobile"`, `"tablet"` (default: `"never"`) |
| `widget.items_alignment` | string | Menu items alignment: `"left"`, `"center"`, `"right"` (default: `"left"`) |
| `widget.items_gap` | number | Gap between menu items in pixels (default: `12`) |
| `widget.items_indent` | number | Left indent for menu items in pixels (default: 0) |
| `widget.link_color` | string | Menu link color (hex) |
| `widget.link_hover_color` | string | Menu link hover color (hex) |
| `widget.link_hover_bg` | string\|null | Menu link hover background color (hex) |
| `widget.link_hover_animation` | string | Hover animation effect: `"none"`, `"underline"`, `"overline"`, `"highlight"`, `"scale"`, `"bracket"` (default: `"none"`) |
| `widget.link_hover_animation_color` | string\|null | Color for the animation element (falls back to `link_hover_color`) |
| `widget.items_padding` | number\|null | Padding for each menu item link in pixels |
| `widget.cta_text` | string | CTA button text |
| `widget.cta_position` | string | CTA position: `"left"`, `"right"`, `"below"` (default: `"right"`) |
| `widget.cta_link_type` | string | CTA link type: `"custom"`, `"page"`, `"entry"` |
| `widget.cta_url` | string | CTA URL (when link type is custom) |
| `widget.cta_page_id` | string | CTA page ID (when link type is page) |
| `widget.cta_collection_code` | string | CTA collection code (when link type is entry) |
| `widget.cta_entry_id` | string | CTA entry ID (when link type is entry) |
| `widget.cta_route_uuid` | string | CTA route UUID |
| `widget.cta_target_blank` | boolean | Open CTA in new tab |
| `widget.cta_style` | string | CTA button style (Bootstrap variants) |
| `widget.cta_size` | string | CTA button size: `"sm"`, `"md"`, `"lg"` |
| `widget.cta_border_radius` | string | CTA button border radius: `"sm"`, `"md"`, `"lg"` (default: `"md"`) |
| `widget.cta_padding` | string\|null | Custom CTA button padding CSS value |
| `widget.cta_icon` | string\|null | CTA button icon class (e.g., `"bx bx-right-arrow-alt"`) |
| `widget.cta_icon_position` | string | CTA icon position: `"left"`, `"right"` (default: `"left"`) |
| `widget.dropdown_bg` | string\|null | Dropdown submenu background color |
| `widget.dropdown_border_radius` | string | Dropdown border radius: `"none"`, `"sm"`, `"md"`, `"lg"` (default: `"md"`) |
| `widget.dropdown_shadow` | string | Dropdown box shadow: `"none"`, `"sm"`, `"md"`, `"lg"` (default: `"lg"`) |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "menu",
  "uuid": "menu-123",
  "widget": {
    "menu_code": "main-nav",
    "label_field": null,
    "logo_light": "https://cdn.example.com/logo.png",
    "logo_dark": "https://cdn.example.com/logo-dark.png",
    "logo_height": 40,
    "logo_position": "left",
    "layout": "horizontal",
    "hamburger_breakpoint": "mobile",
    "items_alignment": "left",
    "items_gap": 12,
    "items_indent": 0,
    "link_color": "#333333",
    "link_hover_color": "#50a5f1",
    "link_hover_bg": null,
    "link_hover_animation": "underline",
    "link_hover_animation_color": "#50a5f1",
    "items_padding": 10,
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
    "cta_size": "md",
    "cta_border_radius": "md",
    "cta_padding": null,
    "cta_icon": null,
    "cta_icon_position": "left",
    "dropdown_bg": null,
    "dropdown_border_radius": "md",
    "dropdown_shadow": "lg"
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

## Hover Animation Values

| Value | Description |
|-------|-------------|
| `none` | No animation (default) |
| `underline` | Line grows from left to right under the link |
| `overline` | Line grows from left to right above the link |
| `highlight` | Background color fades in on hover |
| `scale` | Slight scale up on hover |
| `bracket` | Left/right bracket borders appear on hover |

## Items Gap Values

| Value | Description |
|-------|-------------|
| `sm` | Small gap between items |
| `md` | Medium gap between items |
| `lg` | Large gap between items |

## Dropdown Settings

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `dropdown_bg` | string\|null | `null` | Background color (falls back to white) |
| `dropdown_border_radius` | string | `"md"` | Border radius: `"none"` (0), `"sm"` (4px), `"md"` (8px), `"lg"` (12px) |
| `dropdown_shadow` | string | `"lg"` | Box shadow: `"none"`, `"sm"`, `"md"`, `"lg"` |

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
  } = widget.widget;

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
