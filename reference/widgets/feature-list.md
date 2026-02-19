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
| `content` | object | Widget content |
| `content.items` | array | List of feature items |
| `content.items[].text` | object | Multilingual feature text |
| `content.items[].included` | boolean | Whether feature is included |
| `config` | object | Widget configuration |
| `config.icon_included` | string | Icon class for included items |
| `config.icon_excluded` | string | Icon class for excluded items |
| `config.color_included` | string\|null | Color for included icon |
| `config.color_excluded` | string\|null | Color for excluded icon |
| `config.columns` | number | Number of columns (1-3) |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "feature-list",
  "uuid": "feature-list-123",
  "content": {
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
    ]
  },
  "config": {
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
  const { items } = widget.content;
  const { icon_included, icon_excluded, color_included, color_excluded, columns } = widget.config;

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
