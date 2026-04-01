# Register Form Widget

> **LessCommerce Widget** — This widget requires an active [LessCommerce](https://lesscommerce.io) account connected to your LessCMS project.

A customer registration form for the storefront. Renders fields for creating a new account with configurable options for phone number requirement and post-registration redirect.

## Widget Type

```
register-form
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"register-form"` |
| `uuid` | string | Unique widget identifier |
| `widget.config` | object | Config element group |
| `widget.config.show_login_link` | boolean | Whether to show a link to the login page |
| `widget.config.require_phone` | boolean | Whether the phone number field is required |
| `widget.config.redirect_after_register` | string | URL path to redirect to after successful registration |
| `widget.heading` | object | Heading element group |
| `widget.heading.text` | object | Multilingual heading text |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "register-form",
  "uuid": "rf-606",
  "widget": {
    "config": {
      "show_login_link": true,
      "require_phone": false,
      "redirect_after_register": "/konto"
    },
    "heading": {
      "text": {
        "en": "Create Account",
        "pl": "Utwórz konto"
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
function renderRegisterForm(widget, language) {
  const { config, heading } = widget.widget;
  const title = heading?.text?.[language] || heading?.text?.en || '';

  let html = '';
  if (title) {
    html += `<h2>${title}</h2>`;
  }

  const phoneRequired = config.require_phone ? 'required' : '';

  html += `
    <form class="register-form" action="/auth/register" method="POST">
      <input type="hidden" name="redirect" value="${config.redirect_after_register}" />
      <div class="form-group">
        <label for="name">Name</label>
        <input type="text" id="name" name="name" required />
      </div>
      <div class="form-group">
        <label for="email">Email</label>
        <input type="email" id="email" name="email" required />
      </div>
      <div class="form-group">
        <label for="phone">Phone</label>
        <input type="tel" id="phone" name="phone" ${phoneRequired} />
      </div>
      <div class="form-group">
        <label for="password">Password</label>
        <input type="password" id="password" name="password" required />
      </div>
      <div class="form-group">
        <label for="password_confirmation">Confirm Password</label>
        <input type="password" id="password_confirmation" name="password_confirmation" required />
      </div>
      <button type="submit">Create Account</button>
  `;

  if (config.show_login_link) {
    html += `<a href="/login" class="login-link">Already have an account? Sign in</a>`;
  }

  html += `</form>`;
  return html;
}
```
