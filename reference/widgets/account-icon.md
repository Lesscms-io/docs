# Account Icon Widget

> **LessCommerce Widget** — This widget requires an active [LessCommerce](https://lesscommerce.io) account connected to your LessCMS project.

A user account indicator widget for the storefront header. Displays a user icon with optional name text and links to the account page or login form.

## Widget Type

```
account-icon
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"account-icon"` |
| `uuid` | string | Unique widget identifier |
| `widget.config` | object | Config element group |
| `widget.config.style` | string | Display style: `"icon-text"` or `"icon-only"` |
| `widget.config.show_name` | boolean | Whether to display the user's name when logged in |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "account-icon",
  "uuid": "ai-789",
  "widget": {
    "config": {
      "style": "icon-text",
      "show_name": true
    }
  },
  "settings": {
    "padding_top": 0,
    "padding_bottom": 0,
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Display Styles

| Value | Description |
|-------|-------------|
| `icon-text` | User icon with text label (user name or "Log in") |
| `icon-only` | User icon only, no text |

## Usage Example

```javascript
function renderAccountIcon(widget, user) {
  const { config } = widget.widget;
  const icon = '<i class="fa-solid fa-user"></i>';
  const label = user ? user.name : 'Log in';
  const href = user ? '/account' : '/login';

  if (config.style === 'icon-text') {
    const nameText = config.show_name ? ` <span>${label}</span>` : '';
    return `<a href="${href}" class="account-icon">${icon}${nameText}</a>`;
  }

  return `<a href="${href}" class="account-icon">${icon}</a>`;
}
```
