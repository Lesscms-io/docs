# Feature List Widget

A list of features with included/excluded status indicators. Uses nested element-group structure.

## Widget Type

```
feature-list
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"feature-list"` |
| `uuid` | string | Unique widget identifier |
| `widget.included` | object | Included icon styling |
| `widget.included.icon` | string | Icon class for included items (e.g. `fa-solid fa-check`) |
| `widget.included.color` | string\|null | Color for included icon |
| `widget.included.color:hover` | string\|null | Included icon color on hover |
| `widget.excluded` | object | Excluded icon styling |
| `widget.excluded.icon` | string | Icon class for excluded items (e.g. `fa-solid fa-times`) |
| `widget.excluded.color` | string\|null | Color for excluded icon |
| `widget.excluded.color:hover` | string\|null | Excluded icon color on hover |
| `widget.config` | object | Layout configuration |
| `widget.config.columns` | number | Number of columns (1-3) |
| `widget.items` | array | List of feature entries |
| `widget.items[].html` | object | Multilingual feature text `{ "en": "...", "pl": "..." }` |
| `widget.items[].included` | boolean | Whether feature is included (default: true) |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "feature-list",
  "uuid": "feature-list-123",
  "widget": {
    "included": {
      "icon": "fa-solid fa-check",
      "color": "var:success",
      "color:hover": null
    },
    "excluded": {
      "icon": "fa-solid fa-times",
      "color": "var:danger",
      "color:hover": null
    },
    "config": {
      "columns": 1
    },
    "items": [
      {
        "html": { "en": "Unlimited projects", "pl": "Nieograniczone projekty" },
        "included": true
      },
      {
        "html": { "en": "Custom domains", "pl": "Własne domeny" },
        "included": true
      },
      {
        "html": { "en": "Priority support", "pl": "Priorytetowe wsparcie" },
        "included": false
      }
    ]
  }
}
```

## Usage Example

```javascript
function renderFeatureList(widget, language) {
  const { items, included, excluded, config } = widget.widget;

  const listItems = items.map(item => {
    const text = item.html?.[language] || item.html?.en || '';
    const icon = item.included ? included.icon : excluded.icon;
    const color = item.included ? (included.color || '#28a745') : (excluded.color || '#dc3545');

    return `
      <li style="opacity: ${item.included ? 1 : 0.6}">
        <i class="${icon}" style="color: ${color}; margin-right: 10px;"></i>
        <span>${text}</span>
      </li>
    `;
  }).join('');

  return `
    <ul class="feature-list" style="column-count: ${config.columns || 1}; list-style: none; padding: 0;">
      ${listItems}
    </ul>
  `;
}
```
