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
| `widget` | object | Widget data |
| `widget.items` | array | List of tab items |
| `widget.items[].title` | object | Multilingual tab title |
| `widget.items[].content` | object | Multilingual tab content (HTML) |
| `widget.active_color` | string\|null | Color of active tab |
| `widget.border_color` | string\|null | Border color |
| `widget.style` | string | Tab style: `"underline"`, `"pills"`, `"boxed"` |
| `widget.alignment` | string | Tab alignment: `"left"`, `"center"`, `"right"`, `"stretch"` |
| `widget.hover_active_color` | string\|null | Active tab color on hover |
| `widget.hover_border_color` | string\|null | Border color on hover |
| `widget.transition_duration` | number | Hover transition duration in ms (default: 200) |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "tabs",
  "uuid": "tabs-123",
  "widget": {
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
    ],
    "active_color": "#50a5f1",
    "border_color": "#e0e0e0",
    "style": "underline",
    "alignment": "left",
    "hover_active_color": null,
    "hover_border_color": null,
    "transition_duration": 200
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
  const { items, style, active_color, alignment } = widget.widget;

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
