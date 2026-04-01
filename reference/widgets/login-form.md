# Login Form Widget

> **LessCommerce Widget** — This widget requires an active [LessCommerce](https://lesscommerce.io) account connected to your LessCMS project.

A customer login form for the storefront. Renders email and password fields with configurable helper links and post-login redirect.

## Widget Type

```
login-form
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"login-form"` |
| `uuid` | string | Unique widget identifier |
| `widget.config` | object | Config element group |
| `widget.config.show_register_link` | boolean | Whether to show a link to the registration page |
| `widget.config.show_forgot_password` | boolean | Whether to show a "Forgot password?" link |
| `widget.config.redirect_after_login` | string | URL path to redirect to after successful login |
| `widget.heading` | object | Heading element group |
| `widget.heading.text` | object | Multilingual heading text |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "login-form",
  "uuid": "lf-505",
  "widget": {
    "config": {
      "show_register_link": true,
      "show_forgot_password": true,
      "redirect_after_login": "/konto"
    },
    "heading": {
      "text": {
        "en": "Sign In",
        "pl": "Zaloguj się"
      }
    }
  },
  "settings": {
    "padding_top": 20,
    "padding_bottom": 20,
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Usage Example

```javascript
function renderLoginForm(widget, language) {
  const { config, heading } = widget.widget;
  const title = heading?.text?.[language] || heading?.text?.en || '';

  let html = '';
  if (title) {
    html += `<h2>${title}</h2>`;
  }

  html += `
    <form class="login-form" action="/auth/login" method="POST">
      <input type="hidden" name="redirect" value="${config.redirect_after_login}" />
      <div class="form-group">
        <label for="email">Email</label>
        <input type="email" id="email" name="email" required />
      </div>
      <div class="form-group">
        <label for="password">Password</label>
        <input type="password" id="password" name="password" required />
      </div>
      <button type="submit">Sign In</button>
  `;

  if (config.show_forgot_password) {
    html += `<a href="/forgot-password" class="forgot-link">Forgot password?</a>`;
  }
  if (config.show_register_link) {
    html += `<a href="/register" class="register-link">Create an account</a>`;
  }

  html += `</form>`;
  return html;
}
```
