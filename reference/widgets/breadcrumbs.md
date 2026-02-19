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
| `config` | object | Widget configuration |
| `config.separator` | string | Separator character: `"/"`, `">"`, `">>"`, `"-"`, `"\|"` (default: `"/"`) |
| `config.show_home` | boolean | Show home link as first item (default: true) |
| `config.home_label` | object | Multilingual home label `{ "en": "Home", "pl": "Strona glowna" }` |
| `config.color` | string | Link color (hex) |
| `config.active_color` | string | Active (current page) color (hex) |
| `config.show_dynamic_last` | boolean | Show dynamic last breadcrumb from URL (default: false) |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "breadcrumbs",
  "uuid": "breadcrumbs-123",
  "config": {
    "separator": "/",
    "show_home": true,
    "home_label": {
      "en": "Home",
      "pl": "Strona glowna"
    },
    "color": "#6c757d",
    "active_color": "#212529",
    "show_dynamic_last": false
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
  const { separator, show_home, home_label, color, active_color, show_dynamic_last } = widget.config;

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
