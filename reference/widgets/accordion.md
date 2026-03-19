# Accordion Widget

An expandable/collapsible content widget with multiple items. Uses nested element-group structure.

## Widget Type

```
accordion
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"accordion"` |
| `uuid` | string | Unique widget identifier |
| `widget.header` | object | Header (title) styling |
| `widget.header.color` | string\|null | Title text color |
| `widget.header.color:hover` | string\|null | Title text color on hover |
| `widget.header.background` | string\|null | Header background color |
| `widget.header.background:hover` | string\|null | Header background on hover |
| `widget.header.tag` | string | HTML tag for title (h2-h6, p, span) |
| `widget.content` | object | Content area styling |
| `widget.content.color` | string\|null | Content text color |
| `widget.content.color:hover` | string\|null | Content text color on hover |
| `widget.content.border_color` | string\|null | Content area top border color |
| `widget.content.border_color:hover` | string\|null | Content border color on hover |
| `widget.content.tag` | string | HTML tag for content wrapper (p, div, span) |
| `widget.icon` | object | Expand/collapse icon |
| `widget.icon.color` | string\|null | Icon color |
| `widget.icon.color:hover` | string\|null | Icon color on hover |
| `widget.icon.open` | string | Icon class when item is open (e.g. `fa-solid fa-minus`) |
| `widget.icon.closed` | string | Icon class when item is closed (e.g. `fa-solid fa-plus`) |
| `widget.border` | object | Item border |
| `widget.border.color` | string\|null | Border color around each item |
| `widget.border.color:hover` | string\|null | Item border color on hover |
| `widget.separator` | object | Separator line between items |
| `widget.separator.color` | string\|null | Separator line color |
| `widget.separator.color:hover` | string\|null | Separator color on hover |
| `widget.config` | object | Behavior configuration |
| `widget.config.allow_multiple` | boolean | Allow multiple items open simultaneously |
| `widget.config.first_open` | boolean | First item open by default |
| `widget.items` | array | List of accordion items |
| `widget.items[].title_html` | object | Multilingual item title |
| `widget.items[].html` | object | Multilingual item content |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "accordion",
  "uuid": "acc-001",
  "widget": {
    "header": {
      "color": "var:dark",
      "color:hover": null,
      "background": null,
      "background:hover": null,
      "tag": "h3"
    },
    "content": {
      "color": "var:muted",
      "color:hover": null,
      "border_color": null,
      "border_color:hover": null,
      "tag": "p"
    },
    "icon": {
      "color": "var:primary",
      "color:hover": null,
      "open": "fa-solid fa-minus",
      "closed": "fa-solid fa-plus"
    },
    "border": {
      "color": null,
      "color:hover": null
    },
    "separator": {
      "color": "var:border",
      "color:hover": null
    },
    "config": {
      "allow_multiple": false,
      "first_open": true
    },
    "items": [
      {
        "title_html": {
          "en": "What is LessCMS?",
          "pl": "Czym jest LessCMS?"
        },
        "html": {
          "en": "LessCMS is a headless content management system.",
          "pl": "LessCMS to bezgłowy system zarządzania treścią."
        }
      },
      {
        "title_html": {
          "en": "How to get started?",
          "pl": "Jak zacząć?"
        },
        "html": {
          "en": "Sign up for a free account and create your first project.",
          "pl": "Zarejestruj się za darmo i stwórz swój pierwszy projekt."
        }
      }
    ]
  },
  "settings": {
    "padding_top": 12,
    "padding_bottom": 12,
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Wrappable

No — accordion manages its own items list internally.

## Usage Example

```javascript
function renderAccordion(widget, language) {
  const { header, content, icon, border, separator, config, items } = widget.widget;
  const headerTag = header.tag || 'h3';
  const contentTag = content.tag || 'p';

  return `
    <div class="accordion" data-allow-multiple="${config.allow_multiple}">
      ${items.map((item, index) => {
        const title = item.title_html?.[language] || item.title_html?.en || '';
        const body = item.html?.[language] || item.html?.en || '';
        const isOpen = config.first_open && index === 0;
        const borderStyle = border.color ? `border: 1px solid ${border.color}; border-radius: 8px;` : '';
        const separatorHtml = index > 0 && separator.color
          ? `<div style="height: 1px; background: ${separator.color}"></div>` : '';

        return `
          ${separatorHtml}
          <div class="accordion-item" style="${borderStyle}">
            <button class="accordion-header" aria-expanded="${isOpen}"
              style="color: ${header.color || 'inherit'}; background: ${header.background || 'transparent'}">
              <${headerTag}>${title}</${headerTag}>
              <i class="${isOpen ? icon.open : icon.closed}"
                style="color: ${icon.color || 'inherit'}"></i>
            </button>
            <div class="accordion-body" ${isOpen ? '' : 'hidden'}
              style="color: ${content.color || 'inherit'}; border-top: 1px solid ${content.border_color || 'transparent'}">
              <${contentTag}>${body}</${contentTag}>
            </div>
          </div>
        `;
      }).join('')}
    </div>
  `;
}
```
