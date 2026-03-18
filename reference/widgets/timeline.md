# Timeline Widget

A chronological timeline with date, title and content for each event. Uses nested element-group structure.

## Widget Type

```
timeline
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"timeline"` |
| `uuid` | string | Unique widget identifier |
| `widget.line` | object | Timeline line styling |
| `widget.line.color` | string\|null | Timeline line color |
| `widget.line.color:hover` | string\|null | Timeline line color on hover |
| `widget.dot` | object | Timeline dot styling |
| `widget.dot.color` | string\|null | Timeline dot color |
| `widget.dot.color:hover` | string\|null | Timeline dot color on hover |
| `widget.config` | object | Behavior configuration |
| `widget.config.layout` | string | Layout: `"left"`, `"right"`, `"alternate"` |
| `widget.items` | array | List of timeline events |
| `widget.items[].date` | object | Multilingual date label |
| `widget.items[].title` | object | Multilingual event title |
| `widget.items[].content` | object | Multilingual event description |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "timeline",
  "uuid": "timeline-001",
  "widget": {
    "line": {
      "color": "var:border",
      "color:hover": null
    },
    "dot": {
      "color": "var:primary",
      "color:hover": null
    },
    "config": {
      "layout": "left"
    },
    "items": [
      {
        "date": { "en": "January 2024", "pl": "Styczeń 2024" },
        "title": { "en": "Company Founded", "pl": "Założenie firmy" },
        "content": { "en": "We started with a small team of 3 people.", "pl": "Zaczęliśmy z małym zespołem 3 osób." }
      },
      {
        "date": { "en": "June 2024", "pl": "Czerwiec 2024" },
        "title": { "en": "Product Launch", "pl": "Premiera produktu" },
        "content": { "en": "Our first product went live.", "pl": "Nasz pierwszy produkt został uruchomiony." }
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

## Layout Options

| Value | Description |
|-------|-------------|
| `left` | Items on the left side of the line |
| `right` | Items on the right side of the line |
| `alternate` | Items alternate left and right |

## Usage Example

```javascript
function renderTimeline(widget, language) {
  const { line, dot, config, items } = widget.widget;

  const timelineItems = items.map((item, index) => {
    const date = item.date?.[language] || item.date?.en || '';
    const title = item.title?.[language] || item.title?.en || '';
    const content = item.content?.[language] || item.content?.en || '';
    const side = config.layout === 'alternate' ? (index % 2 === 0 ? 'left' : 'right') : config.layout;

    return `
      <div class="timeline-item timeline-item--${side}">
        <div class="timeline-dot" style="background: ${dot.color || '#50a5f1'}"></div>
        <div class="timeline-card">
          ${date ? `<span class="timeline-date">${date}</span>` : ''}
          ${title ? `<h4>${title}</h4>` : ''}
          ${content ? `<p>${content}</p>` : ''}
        </div>
      </div>
    `;
  }).join('');

  return `
    <div class="timeline timeline--${config.layout}">
      <div class="timeline-line" style="background: ${line.color || '#e0e0e0'}"></div>
      ${timelineItems}
    </div>
  `;
}
```
