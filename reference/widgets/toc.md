# Table of Contents Widget

A navigation widget that renders a list of anchor links pointing to sections on the page. Supports active state highlighting via IntersectionObserver.

## Widget Type

```
toc
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"toc"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget data |
| `widget.toc_title` | object | Multilingual title `{ "en": "Table of Contents", "pl": "Spis tre\u015bci" }` |
| `widget.items` | array | List of ToC items |
| `widget.items[].label` | object | Multilingual item label `{ "en": "Introduction", "pl": "Wprowadzenie" }` |
| `widget.items[].anchor` | string | Target element ID (without `#`) |
| `widget.field_code` | string\|null | Field code to auto-generate TOC from (e.g., richtext field) |
| `widget.heading_level` | string | Heading level to extract from source field (default: `"h2"`) |
| `widget.source_widget_uuid` | string\|null | UUID of the source widget to extract headings from |
| `widget.text_color` | string\|null | Text color for TOC items (hex) |
| `widget.highlight_color` | string\|null | Color for active item highlight (hex, default: `"#50a5f1"`) |
| `widget.show_border` | boolean | Show left border on active item (default: `false`) |
| `widget.title_font_size` | string\|null | Font size for TOC title (e.g., `"18px"`) |
| `widget.title_color` | string\|null | Color for TOC title (hex or color variable) |
| `widget.items_gap` | number\|null | Gap between TOC items in pixels (default: `8`) |
| `widget.font_size` | string\|null | Font size for TOC items (e.g., `"14px"`) |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "toc",
  "uuid": "toc-123",
  "widget": {
    "toc_title": {
      "en": "Table of Contents",
      "pl": "Spis tre\u015bci"
    },
    "items": [
      {
        "label": { "en": "Introduction", "pl": "Wprowadzenie" },
        "anchor": "introduction"
      },
      {
        "label": { "en": "Features", "pl": "Funkcje" },
        "anchor": "features"
      },
      {
        "label": { "en": "Contact", "pl": "Kontakt" },
        "anchor": "contact"
      }
    ],
    "field_code": null,
    "heading_level": "h2",
    "source_widget_uuid": null,
    "text_color": null,
    "highlight_color": "#50a5f1",
    "show_border": true,
    "title_font_size": "18px",
    "title_color": null,
    "items_gap": 8,
    "font_size": null
  },
  "settings": {
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Usage Example

```javascript
function renderToc(widget, language) {
  const { toc_title, items } = widget.widget;
  const { highlight_color, show_border } = widget.widget;

  const title = toc_title?.[language] || toc_title?.en || '';

  const listItems = items.map(item => {
    const label = item.label?.[language] || item.label?.en || '';
    return `
      <li class="toc-item">
        <a href="#${item.anchor}" class="toc-link">${label}</a>
      </li>
    `;
  }).join('');

  return `
    <nav class="toc">
      ${title ? `<div class="toc-title">${title}</div>` : ''}
      <ul class="toc-list">${listItems}</ul>
    </nav>
  `;
}
```

## Active State Tracking

Use `IntersectionObserver` to highlight the currently visible section:

```javascript
const observer = new IntersectionObserver(
  (entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        // Remove active class from all links
        document.querySelectorAll('.toc-link').forEach(link => {
          link.classList.remove('active');
          link.style.color = '';
          link.style.borderLeftColor = '';
        });

        // Add active class to current link
        const activeLink = document.querySelector(
          `.toc-link[href="#${entry.target.id}"]`
        );
        if (activeLink) {
          activeLink.classList.add('active');
          activeLink.style.color = highlight_color;
          if (show_border) {
            activeLink.style.borderLeftColor = highlight_color;
          }
        }
      }
    });
  },
  { rootMargin: '-80px 0px -60% 0px' }
);

// Observe target sections
items.forEach(item => {
  const el = document.getElementById(item.anchor);
  if (el) observer.observe(el);
});
```

## Best Practices

- Place the ToC widget in a **sticky column** so it stays visible while scrolling
- Set target section IDs using the **CSS ID** field in section Advanced settings
- Use descriptive anchor names (e.g., `introduction`, `features`, `contact`)
- Combine with `sticky` column setting and `stickyTop: 100` for fixed navbar clearance
