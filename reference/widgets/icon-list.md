# Icon List Widget

A list of items, each with an icon and text.

## Widget Type

```
icon-list
```

## Data Properties

| Property | Type | Global | Description |
|----------|------|--------|-------------|
| `items` | array | No | Array of list items (multilingual) |

### Item Structure

Each item in the `items` array contains:

| Property | Type | Description |
|----------|------|-------------|
| `icon` | string | Font Awesome icon class |
| `text` | object | Item text (multilingual) |

## Example Response

```json
{
  "widget_type": "icon-list",
  "uuid": "iconlist-123",
  "data": {
    "items": [
      {
        "icon": "fa-solid fa-check",
        "text": {
          "en": "Free shipping on orders over $50",
          "pl": "Darmowa dostawa przy zamówieniach powyżej 200 zł"
        }
      },
      {
        "icon": "fa-solid fa-check",
        "text": {
          "en": "30-day money back guarantee",
          "pl": "30-dniowa gwarancja zwrotu pieniędzy"
        }
      },
      {
        "icon": "fa-solid fa-check",
        "text": {
          "en": "24/7 customer support",
          "pl": "Wsparcie klienta 24/7"
        }
      }
    ]
  },
  "settings": {
    "paddingTop": 16,
    "paddingBottom": 16
  }
}
```

## Usage Example

```javascript
// Render icon list widget
function renderIconList(widget, language) {
  const { items } = widget.data;

  if (!items || items.length === 0) {
    return '';
  }

  const listItems = items.map(item => {
    const text = item.text?.[language] || item.text?.en || '';
    return `
      <li class="icon-list-item">
        <i class="${item.icon}"></i>
        <span>${text}</span>
      </li>
    `;
  }).join('');

  return `<ul class="icon-list">${listItems}</ul>`;
}
```

## Styling Example

```css
.icon-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.icon-list-item {
  display: flex;
  align-items: flex-start;
  gap: 12px;
  padding: 8px 0;
}

.icon-list-item i {
  color: #28a745;
  margin-top: 4px;
}
```
