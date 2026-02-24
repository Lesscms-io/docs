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
| `content` | object | Multilingual content |
| `content.toc_title` | object | Multilingual title `{ "en": "Table of Contents", "pl": "Spis treści" }` |
| `content.items` | array | List of ToC items |
| `content.items[].label` | object | Multilingual item label `{ "en": "Introduction", "pl": "Wprowadzenie" }` |
| `content.items[].anchor` | string | Target element ID (without `#`) |
| `config` | object | Widget configuration |
| `config.highlight_color` | string | Color for active item highlight (hex, default: `"#50a5f1"`) |
| `config.show_border` | boolean | Show left border on active item (default: `false`) |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "toc",
  "uuid": "toc-123",
  "content": {
    "toc_title": {
      "en": "Table of Contents",
      "pl": "Spis treści"
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
    ]
  },
  "config": {
    "highlight_color": "#50a5f1",
    "show_border": true
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
  const { toc_title, items } = widget.content;
  const { highlight_color, show_border } = widget.config;

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
