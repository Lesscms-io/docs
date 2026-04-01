# Breadcrumbs Widget

A navigation breadcrumbs widget showing the current page path with customizable separator and styling.

## Widget Type

```
breadcrumbs
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"breadcrumbs"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget properties |
| `widget.separator` | string | Separator character: `"/"`, `">"`, `">>"`, `"-"`, `"\|"` (default: `"/"`) |
| `widget.show_home` | boolean | Show home link as first item (default: true) |
| `widget.home_label` | object | Multilingual home label `{ "en": "Home", "pl": "Strona glowna" }` |
| `widget.color` | string | Link color (hex) |
| `widget.active_color` | string | Active (current page) color (hex) |
| `widget.show_dynamic_last` | boolean | Show dynamic last breadcrumb from URL (default: false) |
| `widget.dynamic_last_collection_code` | string\|null | Collection code for dynamic last breadcrumb |
| `widget.dynamic_last_field_code` | string\|null | Field code for dynamic last breadcrumb label |
| `widget.dynamic_last_entry_source` | string\|null | Entry source for dynamic last breadcrumb |
| `widget.dynamic_last_entry_id` | string\|null | Entry ID for dynamic last breadcrumb |
| `widget.dynamic_last_url_segment` | string\|null | URL segment for dynamic last breadcrumb |
| `widget.items` | array | Array of breadcrumb items |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "breadcrumbs",
  "uuid": "breadcrumbs-123",
  "widget": {
    "separator": "/",
    "show_home": true,
    "home_label": {
      "en": "Home",
      "pl": "Strona glowna"
    },
    "color": "#6c757d",
    "active_color": "#212529",
    "show_dynamic_last": false,
    "dynamic_last_collection_code": "",
    "dynamic_last_field_code": "",
    "dynamic_last_entry_source": "url",
    "dynamic_last_entry_id": "",
    "dynamic_last_url_segment": 1,
    "items": []
  },
  "settings": {
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Separator Values

| Value | Description |
|-------|-------------|
| `/` | Forward slash |
| `>` | Greater than |
| `>>` | Double greater than |
| `-` | Dash |
| `\|` | Pipe |

## Usage Example

```javascript
function renderBreadcrumbs(widget, language, currentPage, pages) {
  const { separator, show_home, home_label, color, active_color, show_dynamic_last } = widget.widget;

  const homeText = home_label?.[language] || home_label?.en || 'Home';
  const sep = ` <span class="breadcrumb-separator" style="color: ${color};">${separator}</span> `;

  const items = [];

  if (show_home) {
    items.push(`<a href="/" style="color: ${color};">${homeText}</a>`);
  }

  // Build breadcrumb trail from page hierarchy
  if (currentPage?.breadcrumbs) {
    currentPage.breadcrumbs.forEach((crumb, index) => {
      const isLast = index === currentPage.breadcrumbs.length - 1;
      const label = crumb.title?.[language] || crumb.title?.en || '';

      if (isLast) {
        items.push(`<span style="color: ${active_color};">${label}</span>`);
      } else {
        items.push(`<a href="${crumb.url}" style="color: ${color};">${label}</a>`);
      }
    });
  }

  return `
    <nav class="breadcrumbs" aria-label="Breadcrumb">
      ${items.join(sep)}
    </nav>
  `;
}
```
