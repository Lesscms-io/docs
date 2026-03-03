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
| `widget` | object | Widget data |
| `widget.items` | array | List of accordion items |
| `widget.items[].title` | object | Multilingual item title |
| `widget.items[].content` | object | Multilingual item content (HTML) |
| `widget.icon_color` | string\|null | Color of expand/collapse icon |
| `widget.border_color` | string\|null | Border color of items |
| `widget.allow_multiple` | boolean | Allow multiple items open at once |
| `widget.first_open` | boolean | First item open by default |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "accordion",
  "uuid": "accordion-123",
  "widget": {
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
    ],
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
  const { items } = widget.widget;
  const { allow_multiple, first_open, icon_color, border_color } = widget.widget;

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
