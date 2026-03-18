# Table of Contents Widget

A navigation widget that renders a list of anchor links pointing to sections on the page. Uses element-group structure. Supports active state highlighting via IntersectionObserver.

## Widget Type

```
toc
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"toc"` |
| `uuid` | string | Unique widget identifier |
| `widget.title` | object | Title element group |
| `widget.title.text` | object | Multilingual title `{ "en": "Table of Contents", "pl": "Spis treści" }` |
| `widget.title.color` | string\|null | Title text color (color variable or hex) |
| `widget.title.color:hover` | string\|null | Title text color on hover |
| `widget.title.font_size` | number\|null | Title font size in pixels |
| `widget.text` | object | Text/items styling group |
| `widget.text.color` | string\|null | Text color for TOC items |
| `widget.text.color:hover` | string\|null | Item text color on hover |
| `widget.text.font_size` | number\|null | Font size for TOC items in pixels |
| `widget.text.items_gap` | number | Gap between TOC items in pixels (default: `8`) |
| `widget.highlight` | object | Active item highlight group |
| `widget.highlight.color` | string\|null | Color for active item highlight (default: `"var:info"`) |
| `widget.highlight.color:hover` | string\|null | Highlight color on hover |
| `widget.config` | object | Behavior configuration |
| `widget.config.heading_level` | string | Heading level to extract (default: `"h2"`) |
| `widget.config.field_code` | string | Field code to auto-generate TOC from (e.g., richtext field) |
| `widget.config.source_widget_uuid` | string | UUID of the source widget to extract headings from |
| `widget.config.show_border` | boolean | Show left border on active item (default: `true`) |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "toc",
  "uuid": "toc-123",
  "widget": {
    "title": {
      "text": {
        "en": "Table of Contents",
        "pl": "Spis treści"
      },
      "color": null,
      "color:hover": null,
      "font_size": null
    },
    "text": {
      "color": null,
      "color:hover": null,
      "font_size": null,
      "items_gap": 8
    },
    "highlight": {
      "color": "var:info",
      "color:hover": null
    },
    "config": {
      "heading_level": "h2",
      "field_code": "",
      "source_widget_uuid": "",
      "show_border": true
    }
  },
  "settings": {
    "padding_top": 0,
    "padding_bottom": 0,
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
  const { title, text, highlight, config } = widget.widget;

  const tocTitle = title.text?.[language] || title.text?.en || '';
  const textColor = text.color || '#495057';
  const highlightColor = highlight.color || '#50a5f1';
  const itemsGap = text.items_gap ?? 8;
  const showBorder = config.show_border !== false;

  // Auto-generate items from DOM headings
  const tag = config.heading_level || 'h2';
  const headings = document.querySelectorAll(tag);
  const items = Array.from(headings).map(el => ({
    label: el.textContent.trim(),
    anchor: el.id || slugify(el.textContent.trim())
  })).filter(i => i.label);

  const listItems = items.map(item => `
    <li class="toc-item">
      <a href="#${item.anchor}" class="toc-link"
         style="color: ${textColor}; font-size: ${text.font_size ? text.font_size + 'px' : 'inherit'}">
        ${item.label}
      </a>
    </li>
  `).join('');

  return `
    <nav class="toc">
      ${tocTitle ? `<div class="toc-title" style="color: ${title.color || 'inherit'}; font-size: ${title.font_size ? title.font_size + 'px' : 'inherit'}">${tocTitle}</div>` : ''}
      <ul class="toc-list" style="gap: ${itemsGap}px; ${showBorder ? 'border-left: 3px solid ' + highlightColor : ''}">${listItems}</ul>
    </nav>
  `;
}
```

## Active State Tracking

Use `IntersectionObserver` to highlight the currently visible section:

```javascript
const { highlight, config } = widget.widget;
const highlightColor = highlight.color || '#50a5f1';
const showBorder = config.show_border !== false;

const observer = new IntersectionObserver(
  (entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        document.querySelectorAll('.toc-link').forEach(link => {
          link.classList.remove('active');
          link.style.color = '';
          link.style.borderLeftColor = '';
        });

        const activeLink = document.querySelector(
          `.toc-link[href="#${entry.target.id}"]`
        );
        if (activeLink) {
          activeLink.classList.add('active');
          activeLink.style.color = highlightColor;
          if (showBorder) {
            activeLink.style.borderLeftColor = highlightColor;
          }
        }
      }
    });
  },
  { rootMargin: '-80px 0px -60% 0px' }
);
```

## Best Practices

- Place the ToC widget in a **sticky column** so it stays visible while scrolling
- Set target section IDs using the **CSS ID** field in section Advanced settings
- Use descriptive anchor names (e.g., `introduction`, `features`, `contact`)
- Combine with `sticky` column setting and `stickyTop: 100` for fixed navbar clearance
