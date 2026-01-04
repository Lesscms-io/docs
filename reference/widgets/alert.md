# Alert Widget

A notification/alert box with title, message, and customizable type.

## Widget Type

```
alert
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"alert"` |
| `uuid` | string | Unique widget identifier |
| `content` | object | Widget content |
| `content.title` | object | Multilingual alert title |
| `content.message` | object | Multilingual alert message |
| `config` | object | Widget configuration |
| `config.type` | string | Alert type: `"info"`, `"success"`, `"warning"`, `"danger"` |
| `config.dismissible` | boolean | Allow user to dismiss/close the alert |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "alert",
  "uuid": "alert-123",
  "content": {
    "title": {
      "en": "Important Notice",
      "pl": "Ważna informacja"
    },
    "message": {
      "en": "Our office will be closed on December 25th for the holiday.",
      "pl": "Nasze biuro będzie zamknięte 25 grudnia z powodu święta."
    }
  },
  "config": {
    "type": "info",
    "dismissible": true
  },
  "settings": {
    "marginTop": 16,
    "marginBottom": 16,
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Alert Type Values

| Value | Description | Typical Color |
|-------|-------------|---------------|
| `info` | Informational message | Blue |
| `success` | Success/confirmation | Green |
| `warning` | Warning/caution | Yellow/Orange |
| `danger` | Error/critical | Red |

## Usage Example

```javascript
function renderAlert(widget, language) {
  const { title, message } = widget.content;
  const { type, dismissible } = widget.config;

  const titleText = title?.[language] || title?.en || '';
  const messageText = message?.[language] || message?.en || '';
  const alertType = type || 'info';

  return `
    <div class="alert alert-${alertType}" role="alert">
      <div class="alert-body">
        ${titleText ? `<strong class="alert-title">${titleText}</strong>` : ''}
        ${messageText ? `<p class="alert-message">${messageText}</p>` : ''}
      </div>
      ${dismissible ? `
        <button class="alert-dismiss" onclick="this.parentElement.remove()" aria-label="Close">×</button>
      ` : ''}
    </div>
  `;
}
```
