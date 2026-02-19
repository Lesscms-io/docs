# Timeline Widget

A chronological timeline with date, title and content for each event.

## Widget Type

```
timeline
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"timeline"` |
| `uuid` | string | Unique widget identifier |
| `content` | object | Widget content |
| `content.items` | array | List of timeline events |
| `content.items[].date` | object | Multilingual date label |
| `content.items[].title` | object | Multilingual event title |
| `content.items[].content` | object | Multilingual event description |
| `config` | object | Widget configuration |
| `config.layout` | string | Layout: `"left"`, `"right"`, `"alternate"` |
| `config.line_color` | string\|null | Timeline line color |
| `config.dot_color` | string\|null | Timeline dot color |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "timeline",
  "uuid": "timeline-123",
  "content": {
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
  "config": {
    "layout": "left",
    "line_color": "#e0e0e0",
    "dot_color": "#50a5f1"
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
  const { items } = widget.content;
  const { layout, line_color, dot_color } = widget.config;

  const timelineItems = items.map((item, index) => {
    const date = item.date?.[language] || item.date?.en || '';
    const title = item.title?.[language] || item.title?.en || '';
    const content = item.content?.[language] || item.content?.en || '';
    const side = layout === 'alternate' ? (index % 2 === 0 ? 'left' : 'right') : layout;

    return `
      <div class="timeline-item timeline-item--${side}">
        <div class="timeline-dot" style="background: ${dot_color || '#50a5f1'}"></div>
        <div class="timeline-card">
          ${date ? `<span class="timeline-date">${date}</span>` : ''}
          ${title ? `<h4>${title}</h4>` : ''}
          ${content ? `<p>${content}</p>` : ''}
        </div>
      </div>
    `;
  }).join('');

  return `
    <div class="timeline timeline--${layout}">
      <div class="timeline-line" style="background: ${line_color || '#e0e0e0'}"></div>
      ${timelineItems}
    </div>
  `;
}
```
