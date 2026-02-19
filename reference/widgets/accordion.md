# Accordion Widget

An expandable/collapsible content widget with multiple items.

## Widget Type

```
accordion
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"accordion"` |
| `uuid` | string | Unique widget identifier |
| `content` | object | Widget content |
| `content.items` | array | List of accordion items |
| `content.items[].title` | object | Multilingual item title |
| `content.items[].content` | object | Multilingual item content (HTML) |
| `config` | object | Widget configuration |
| `config.icon_color` | string\|null | Color of expand/collapse icon |
| `config.border_color` | string\|null | Border color of items |
| `config.allow_multiple` | boolean | Allow multiple items open at once |
| `config.first_open` | boolean | First item open by default |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "accordion",
  "uuid": "accordion-123",
  "content": {
    "items": [
      {
        "title": {
          "en": "What is LessCMS?",
          "pl": "Czym jest LessCMS?"
        },
        "content": {
          "en": "<p>LessCMS is a headless content management system.</p>",
          "pl": "<p>LessCMS to bezgłowy system zarządzania treścią.</p>"
        }
      },
      {
        "title": {
          "en": "How to get started?",
          "pl": "Jak zacząć?"
        },
        "content": {
          "en": "<p>Sign up for a free account and create your first project.</p>",
          "pl": "<p>Zarejestruj się za darmo i stwórz swój pierwszy projekt.</p>"
        }
      }
    ]
  },
  "config": {
    "icon_color": "#50a5f1",
    "border_color": "#e0e0e0",
    "allow_multiple": false,
    "first_open": true
  }
}
```

## Usage Example

```javascript
function renderAccordion(widget, language) {
  const { items } = widget.content;
  const { allow_multiple, first_open, icon_color, border_color } = widget.config;

  return `
    <div class="accordion" data-allow-multiple="${allow_multiple}">
      ${items.map((item, index) => {
        const title = item.title?.[language] || item.title?.en || '';
        const content = item.content?.[language] || item.content?.en || '';
        const isOpen = first_open && index === 0;

        return `
          <div class="accordion-item" style="border-color: ${border_color || '#e0e0e0'}">
            <button class="accordion-header" aria-expanded="${isOpen}">
              <span>${title}</span>
              <i class="fa-solid fa-chevron-down" style="color: ${icon_color || 'inherit'}"></i>
            </button>
            <div class="accordion-body" ${isOpen ? '' : 'hidden'}>${content}</div>
          </div>
        `;
      }).join('')}
    </div>
  `;
}
```
