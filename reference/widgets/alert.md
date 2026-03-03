# Alert Widget

A notification/alert box with title, content, and customizable type.

## Widget Type

```
alert
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"alert"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget properties |
| `widget.title` | object | Multilingual alert title |
| `widget.message` | object | Multilingual alert message text |
| `widget.type` | string | Alert type: `"info"`, `"success"`, `"warning"`, `"danger"`, `"custom"` |
| `widget.dismissible` | boolean | Allow user to dismiss/close the alert |
| `widget.show_title` | boolean | Show the alert title (default: true) |
| `widget.icon` | string\|null | Custom icon class (Font Awesome), used when type is `"custom"` |
| `widget.background_color` | string\|null | Custom background color, used when type is `"custom"` |
| `widget.border_color` | string\|null | Custom border color, used when type is `"custom"` |
| `widget.text_color` | string\|null | Custom text color, used when type is `"custom"` |
| `settings` | object | Style settings (optional) |

## Example Response (Preset Type)

```json
{
  "widget_type": "alert",
  "uuid": "alert-123",
  "widget": {
    "title": {
      "en": "Important Notice",
      "pl": "Wazna informacja"
    },
    "message": {
      "en": "Our office will be closed on December 25th for the holiday.",
      "pl": "Nasze biuro bedzie zamkniete 25 grudnia z powodu swieta."
    },
    "type": "info",
    "dismissible": true,
    "show_title": true,
    "icon": null,
    "background_color": null,
    "border_color": null,
    "text_color": null
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

## Example Response (Custom Type)

```json
{
  "widget_type": "alert",
  "uuid": "alert-456",
  "widget": {
    "title": {
      "en": "Maintenance Scheduled",
      "pl": "Planowana przerwa techniczna"
    },
    "message": {
      "en": "System maintenance is scheduled for Saturday 2:00 AM - 4:00 AM.",
      "pl": "Przerwa techniczna zaplanowana na sobote 2:00 - 4:00."
    },
    "type": "custom",
    "dismissible": false,
    "show_title": true,
    "icon": "fa-solid fa-wrench",
    "background_color": "#fff3cd",
    "border_color": "#ffc107",
    "text_color": "#856404"
  },
  "settings": {}
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
  const { title, message, type, dismissible } = widget.widget;

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
        <button class="alert-dismiss" onclick="this.parentElement.remove()" aria-label="Close">&times;</button>
      ` : ''}
    </div>
  `;
}
```
