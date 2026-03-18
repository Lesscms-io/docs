# Tabs Widget

Tabbed content interface with configurable styles. Uses nested element-group structure.

## Widget Type

```
tabs
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"tabs"` |
| `uuid` | string | Unique widget identifier |
| `widget.tab` | object | Tab styling |
| `widget.tab.color` | string\|null | Active tab text/border color |
| `widget.tab.color:hover` | string\|null | Active tab color on hover |
| `widget.tab.border` | string\|null | Tab list border color |
| `widget.tab.border:hover` | string\|null | Tab list border color on hover |
| `widget.tab.style` | string | Tab style: `"underline"`, `"pills"`, `"boxed"` |
| `widget.config` | object | Behavior configuration |
| `widget.config.alignment` | string | Tab alignment: `"left"`, `"center"`, `"right"`, `"stretch"` |
| `widget.items` | array | List of tab items |
| `widget.items[].title` | object | Multilingual tab title |
| `widget.items[].content` | object | Multilingual tab content (HTML) |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "tabs",
  "uuid": "tabs-123",
  "widget": {
    "tab": {
      "color": "var:primary",
      "color:hover": null,
      "border": "var:border",
      "border:hover": null,
      "style": "underline"
    },
    "config": {
      "alignment": "left"
    },
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
  "settings": {
    "padding_top": 12,
    "padding_bottom": 12,
    "padding_left": 0,
    "padding_right": 0,
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
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
  const { tab, config, items } = widget.widget;

  const tabHeaders = items.map((item, index) => {
    const title = item.title?.[language] || item.title?.en || '';
    return `<button class="tab-btn ${index === 0 ? 'active' : ''}"
              data-tab="${index}"
              style="${index === 0 && tab.color ? `color: ${tab.color}; border-color: ${tab.color}` : ''}">
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
    <div class="tabs tabs--${tab.style}" style="text-align: ${config.alignment}">
      <div class="tab-list" style="${tab.border ? `border-color: ${tab.border}` : ''}">${tabHeaders}</div>
      <div class="tab-content">${tabPanels}</div>
    </div>
  `;
}
```
