# Alert Widget

A notification/alert box with title, content, and customizable type.

## Widget Type

```
alert
```

## Data Properties

| Property | Type | Global | Description |
|----------|------|--------|-------------|
| `title` | object | No | Alert title (multilingual) |
| `content` | object | No | Alert message content (multilingual) |
| `type` | string | Yes | Alert type: `info`, `success`, `warning`, `danger` |
| `dismissible` | boolean | Yes | Allow user to dismiss/close the alert |

## Example Response

```json
{
  "widget_type": "alert",
  "uuid": "alert-123",
  "data": {
    "title": {
      "en": "Important Notice",
      "pl": "Ważna informacja"
    },
    "content": {
      "en": "Our office will be closed on December 25th for the holiday. We will resume normal business hours on December 26th.",
      "pl": "Nasze biuro będzie zamknięte 25 grudnia z powodu święta. Wracamy do normalnych godzin pracy 26 grudnia."
    },
    "type": "info",
    "dismissible": true
  },
  "settings": {
    "marginTop": 16,
    "marginBottom": 16
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

## Example Responses by Type

### Info Alert

```json
{
  "type": "info",
  "title": { "en": "Did you know?" },
  "content": { "en": "You can customize your dashboard in the settings." }
}
```

### Success Alert

```json
{
  "type": "success",
  "title": { "en": "Order Confirmed!" },
  "content": { "en": "Your order #12345 has been successfully placed." }
}
```

### Warning Alert

```json
{
  "type": "warning",
  "title": { "en": "Action Required" },
  "content": { "en": "Your subscription will expire in 7 days. Renew now to avoid interruption." }
}
```

### Danger Alert

```json
{
  "type": "danger",
  "title": { "en": "Payment Failed" },
  "content": { "en": "We couldn't process your payment. Please update your billing information." }
}
```

## Usage Example

```javascript
// Render alert widget
function renderAlert(widget, language) {
  const { title, content, type, dismissible } = widget.data;

  const titleText = title?.[language] || title?.en || '';
  const contentText = content?.[language] || content?.en || '';
  const alertType = type || 'info';

  const alertId = `alert-${widget.uuid}`;

  const icons = {
    info: 'fa-circle-info',
    success: 'fa-circle-check',
    warning: 'fa-triangle-exclamation',
    danger: 'fa-circle-exclamation'
  };

  return `
    <div id="${alertId}" class="alert-widget alert-${alertType}" role="alert">
      <i class="alert-icon fa-solid ${icons[alertType]}"></i>
      <div class="alert-body">
        ${titleText ? `<strong class="alert-title">${titleText}</strong>` : ''}
        ${contentText ? `<p class="alert-content">${contentText}</p>` : ''}
      </div>
      ${dismissible ? `
        <button class="alert-dismiss" onclick="this.parentElement.remove()" aria-label="Close">
          <i class="fa-solid fa-xmark"></i>
        </button>
      ` : ''}
    </div>
  `;
}
```

## CSS Example

```css
.alert-widget {
  display: flex;
  align-items: flex-start;
  gap: 12px;
  padding: 16px;
  border-radius: 8px;
  margin: 16px 0;
}

.alert-icon {
  font-size: 20px;
  margin-top: 2px;
}

.alert-body {
  flex: 1;
}

.alert-title {
  display: block;
  margin-bottom: 4px;
}

.alert-content {
  margin: 0;
  font-size: 14px;
}

.alert-dismiss {
  background: none;
  border: none;
  cursor: pointer;
  padding: 4px;
  opacity: 0.6;
}

.alert-dismiss:hover {
  opacity: 1;
}

/* Alert Types */
.alert-info {
  background: #E7F3FF;
  border: 1px solid #B6D4FE;
  color: #084298;
}

.alert-success {
  background: #D1E7DD;
  border: 1px solid #BADBCC;
  color: #0F5132;
}

.alert-warning {
  background: #FFF3CD;
  border: 1px solid #FFECB5;
  color: #664D03;
}

.alert-danger {
  background: #F8D7DA;
  border: 1px solid #F5C2C7;
  color: #842029;
}
```
