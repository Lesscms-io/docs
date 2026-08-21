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
| `widget.config.show_relative` | boolean | Render a "how long ago" line for each item (computed from `date_from` to now) |
| `widget.config.show_duration` | boolean | Render a "how long it lasted" line for items that have a `date_to` |
| `widget.items` | array | List of timeline events |
| `widget.items[].date_from` | string\|null | Structured event date / range start, ISO `YYYY-MM-DD` |
| `widget.items[].date_to` | string\|null | Optional range end, ISO `YYYY-MM-DD` |
| `widget.items[].date_html` | object | Optional custom label (multilingual). When empty, the header is auto-formatted from `date_from`/`date_to` |
| `widget.items[].title_html` | object | Multilingual event title |
| `widget.items[].html` | object | Multilingual event description |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Computed date fields

`date_from` / `date_to` are plain ISO day strings. `date_html` is now an
**optional custom label** — leave it empty to let the client auto-format the
header from the structured dates.

When `config.show_relative` is `true`, render the elapsed time from `date_from`
to the current date, e.g. `2 lata i 3 miesiące temu (830 dni)`.

When `config.show_duration` is `true` and an item has `date_to`, render the
range length, e.g. `1 rok, 2 miesiące i 4 dni (432 dni)`. Both use a simplified
calendar breakdown (years/months/days) with the total day count in brackets.
These strings are computed client-side (the API only ships the raw dates and
the two boolean flags), so localize the wording to the render language.

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
      "layout": "left",
      "show_relative": true,
      "show_duration": true
    },
    "items": [
      {
        "date_from": "2024-01-15",
        "date_to": null,
        "date_html": { "en": "January 2024", "pl": "Styczeń 2024" },
        "title_html": { "en": "Company Founded", "pl": "Założenie firmy" },
        "html": { "en": "We started with a small team of 3 people.", "pl": "Zaczęliśmy z małym zespołem 3 osób." }
      },
      {
        "date_from": "2024-06-01",
        "date_to": "2025-08-07",
        "date_html": {},
        "title_html": { "en": "Product Launch", "pl": "Premiera produktu" },
        "html": { "en": "Our first product went live.", "pl": "Nasz pierwszy produkt został uruchomiony." }
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
    const date = item.date_html?.[language] || item.date_html?.en || '';
    const title = item.title_html?.[language] || item.title_html?.en || '';
    const content = item.html?.[language] || item.html?.en || '';
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
