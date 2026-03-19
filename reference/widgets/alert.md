# Alert Widget

A notification/alert box with title, content, and customizable type. Uses nested element-group structure.

## Widget Type

```
alert
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"alert"` |
| `uuid` | string | Unique widget identifier |
| `widget.icon` | object | Icon element |
| `widget.icon.icon` | string\|null | Custom icon class (Font Awesome) |
| `widget.title` | object | Title element |
| `widget.title.html` | object | Multilingual alert title |
| `widget.title.color` | string\|null | Title text color (CSS or `"var:colorName"`) |
| `widget.title.color:hover` | string\|null | Title text color on hover |
| `widget.text` | object | Text element |
| `widget.text.html` | object | Multilingual alert message text |
| `widget.text.color` | string\|null | Text color |
| `widget.text.color:hover` | string\|null | Text color on hover |
| `widget.config` | object | Configuration element |
| `widget.config.type` | string | Alert type: `"info"`, `"success"`, `"warning"`, `"danger"` |
| `widget.config.show_title` | boolean | Show the alert title (default: true) |
| `widget.config.dismissible` | boolean | Allow user to dismiss/close the alert (default: false) |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Alert Type Values

| Value | Description | Typical Color |
|-------|-------------|---------------|
| `info` | Informational message | Blue |
| `success` | Success/confirmation | Green |
| `warning` | Warning/caution | Yellow/Orange |
| `danger` | Error/critical | Red |

## Example Response

```json
{
  "widget_type": "alert",
  "uuid": "alert-123",
  "widget": {
    "icon": {
      "icon": "fa-solid fa-circle-info"
    },
    "title": {
      "html": {
        "en": "Important Notice",
        "pl": "Wazna informacja"
      }
    },
    "text": {
      "html": {
        "en": "Our office will be closed on December 25th for the holiday.",
        "pl": "Nasze biuro bedzie zamkniete 25 grudnia z powodu swieta."
      },
      "color": null,
      "color:hover": null
    },
    "config": {
      "type": "info",
      "show_title": true,
      "dismissible": false
    }
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

## Usage Example

```javascript
function renderAlert(widget, language) {
  const { icon, title, text, config } = widget.widget;

  const titleText = title.html?.[language] || title.html?.en || '';
  const messageText = text.html?.[language] || text.html?.en || '';
  const alertType = config.type || 'info';
  const showTitle = config.show_title !== false;
  const dismissible = config.dismissible || false;

  const iconClass = icon.icon || {
    info: 'fa-solid fa-circle-info',
    success: 'fa-solid fa-circle-check',
    warning: 'fa-solid fa-triangle-exclamation',
    danger: 'fa-solid fa-circle-xmark'
  }[alertType] || 'fa-solid fa-circle-info';

  return `
    <div class="alert alert-${alertType}" role="alert">
      <div class="alert-icon">
        <i class="${iconClass}"></i>
      </div>
      <div class="alert-body">
        ${showTitle && titleText ? `<strong class="alert-title">${titleText}</strong>` : ''}
        ${messageText ? `<p class="alert-message">${messageText}</p>` : ''}
      </div>
      ${dismissible ? `
        <button class="alert-dismiss" onclick="this.parentElement.remove()" aria-label="Close">&times;</button>
      ` : ''}
    </div>
  `;
}
```
