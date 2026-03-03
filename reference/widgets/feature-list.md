# Feature List Widget

A list of features with included/excluded status indicators.

## Widget Type

```
feature-list
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"feature-list"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget data |
| `widget.items` | array | List of feature items |
| `widget.items[].text` | object | Multilingual feature text `{ "en": "...", "pl": "..." }` |
| `widget.items[].included` | boolean | Whether feature is included (default: true) |
| `widget.icon_included` | string | Icon class for included items |
| `widget.icon_excluded` | string | Icon class for excluded items |
| `widget.color_included` | string\|null | Color for included icon |
| `widget.color_excluded` | string\|null | Color for excluded icon |
| `widget.columns` | number | Number of columns (1-3) |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "feature-list",
  "uuid": "feature-list-123",
  "widget": {
    "items": [
      {
        "text": { "en": "Unlimited projects", "pl": "Nieograniczone projekty" },
        "included": true
      },
      {
        "text": { "en": "Custom domains", "pl": "Własne domeny" },
        "included": true
      },
      {
        "text": { "en": "Priority support", "pl": "Priorytetowe wsparcie" },
        "included": false
      }
    ],
    "icon_included": "fa-solid fa-check",
    "icon_excluded": "fa-solid fa-xmark",
    "color_included": "#28a745",
    "color_excluded": "#dc3545",
    "columns": 1
  }
}
```

## Usage Example

```javascript
function renderFeatureList(widget, language) {
  const { items, icon_included, icon_excluded, color_included, color_excluded, columns } = widget.widget;

  const listItems = items.map(item => {
    const text = item.text?.[language] || item.text?.en || '';
    const icon = item.included ? icon_included : icon_excluded;
    const color = item.included ? (color_included || '#28a745') : (color_excluded || '#dc3545');

    return `
      <li style="opacity: ${item.included ? 1 : 0.6}">
        <i class="${icon}" style="color: ${color}; margin-right: 10px;"></i>
        <span>${text}</span>
      </li>
    `;
  }).join('');

  return `
    <ul class="feature-list" style="column-count: ${columns || 1}; list-style: none; padding: 0;">
      ${listItems}
    </ul>
  `;
}
```
