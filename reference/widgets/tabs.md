# Tabs Widget

Tabbed content interface with configurable styles.

## Widget Type

```
tabs
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"tabs"` |
| `uuid` | string | Unique widget identifier |
| `content` | object | Widget content |
| `content.items` | array | List of tab items |
| `content.items[].title` | object | Multilingual tab title |
| `content.items[].content` | object | Multilingual tab content (HTML) |
| `config` | object | Widget configuration |
| `config.active_color` | string\|null | Color of active tab |
| `config.border_color` | string\|null | Border color |
| `config.style` | string | Tab style: `"underline"`, `"pills"`, `"boxed"` |
| `config.alignment` | string | Tab alignment: `"left"`, `"center"`, `"right"`, `"stretch"` |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "tabs",
  "uuid": "tabs-123",
  "content": {
    "items": [
      {
        "title": {
          "en": "Features",
          "pl": "Funkcje"
        },
        "content": {
          "en": "<p>Discover our powerful features.</p>",
          "pl": "<p>Odkryj nasze funkcje.</p>"
        }
      },
      {
        "title": {
          "en": "Pricing",
          "pl": "Cennik"
        },
        "content": {
          "en": "<p>Simple and transparent pricing.</p>",
          "pl": "<p>Prosty i przejrzysty cennik.</p>"
        }
      }
    ]
  },
  "config": {
    "active_color": "#50a5f1",
    "border_color": "#e0e0e0",
    "style": "underline",
    "alignment": "left"
  }
}
```

## Tab Styles

| Value | Description |
|-------|-------------|
| `underline` | Active tab has underline border |
| `pills` | Active tab has pill/rounded background |
| `boxed` | Active tab has boxed border |

## Usage Example

```javascript
function renderTabs(widget, language) {
  const { items } = widget.content;
  const { style, active_color, alignment } = widget.config;

  const tabHeaders = items.map((item, index) => {
    const title = item.title?.[language] || item.title?.en || '';
    return `<button class="tab-btn ${index === 0 ? 'active' : ''}"
              data-tab="${index}"
              style="${index === 0 && active_color ? `color: ${active_color}; border-color: ${active_color}` : ''}">
              ${title}
            </button>`;
  }).join('');

  const tabPanels = items.map((item, index) => {
    const content = item.content?.[language] || item.content?.en || '';
    return `<div class="tab-panel ${index === 0 ? 'active' : ''}" data-panel="${index}">
              ${content}
            </div>`;
  }).join('');

  return `
    <div class="tabs tabs--${style}" style="text-align: ${alignment}">
      <div class="tab-list">${tabHeaders}</div>
      <div class="tab-content">${tabPanels}</div>
    </div>
  `;
}
```
