# Form Widget

A form widget that supports two modes: referencing a system form by code (new mode), or inline field definitions (legacy mode).

## Widget Type

```
form
```

## Response Structure (Form Code Mode)

When `form_code` is set, the widget references a system form. The frontend fetches the form definition separately via the Forms API.

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"form"` |
| `uuid` | string | Unique widget identifier |
| `config` | object | Widget configuration |
| `config.form_code` | string | Code of the form to render (references Forms API) |
| `config.submit_text` | string | Submit button text |
| `config.button_style` | string | Submit button CSS style (default: `"info"`) |
| `config.button_size` | string | Submit button size (default: `"md"`) |
| `config.button_border_radius` | string | Button border radius (default: `"md"`) |
| `config.button_padding` | string | Button padding |
| `config.button_icon` | string | Button icon class |
| `config.button_icon_position` | string | Icon position: `"left"` or `"right"` (default: `"left"`) |
| `config.button_align` | string | Button alignment: `"left"`, `"center"`, `"right"` |
| `config.label_position` | string | Label position: `"top"`, `"side"` |
| `config.columns` | string | Form columns: `"1"`, `"2"` |
| `config.input_size` | string | Input size: `"sm"`, `"md"`, `"lg"` |
| `config.input_border_radius` | string | Input border radius: `"none"`, `"sm"`, `"md"`, `"lg"`, `"pill"` |
| `config.input_padding` | string | Input padding in px |
| `config.input_background_color` | string | Input background color (hex or `"var:primary"` variable reference) |
| `config.input_text_color` | string | Input text color |
| `config.input_border_color` | string | Input border color |
| `config.input_border_width` | string | Input border width in px |
| `config.input_border_style` | string | Input border style: `"solid"`, `"dashed"`, `"dotted"` |
| `config.input_focus_border_color` | string | Input border color on focus |
| `config.input_placeholder_color` | string | Input placeholder text color |
| `settings` | object | Style settings (optional) |

## Response Structure (Legacy Inline Fields Mode)

When `form_code` is not set, the widget uses inline field definitions. This is the backward-compatible mode.

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"form"` |
| `uuid` | string | Unique widget identifier |
| `config` | object | Widget configuration |
| `config.form_uuid` | string | Form identifier (same as widget uuid) |
| `config.fields` | array | Array of form field definitions |
| `config.fields[].code` | string | Field code/name |
| `config.code` | string | Alias for per-field code (inside each field object) |
| `config.fields[].type` | string | Field type: `"text"`, `"email"`, `"textarea"`, `"select"`, etc. |
| `config.type` | string | Alias for per-field type (inside each field object) |
| `config.fields[].label` | string | Field label |
| `config.label` | string | Alias for per-field label (inside each field object) |
| `config.fields[].placeholder` | string | Field placeholder text |
| `config.placeholder` | string | Alias for per-field placeholder (inside each field object) |
| `config.fields[].required` | boolean | Whether the field is required (default: false) |
| `config.required` | boolean | Alias for per-field required flag (inside each field object) |
| `config.fields[].options` | array | Options for select/radio fields |
| `config.options` | array | Alias for per-field options (inside each field object) |
| `config.submit_text` | string | Submit button text (default: `"Submit"`) |
| `config.success_message` | string | Message shown after successful submission |
| `config.error_message` | string | Message shown on submission error |
| `config.button_color` | string | Button background color |
| `config.email_to` | string | Email address to send submissions to |
| `settings` | object | Style settings (optional) |

## Example Response (Form Code Mode)

```json
{
  "widget_type": "form",
  "uuid": "form-widget-123",
  "config": {
    "form_code": "contact",
    "submit_text": "Send Message",
    "button_style": "info",
    "button_size": "md",
    "button_border_radius": "md",
    "button_padding": "",
    "button_icon": "",
    "button_icon_position": "left",
    "button_align": "left",
    "label_position": "top",
    "columns": "1",
    "input_size": "md",
    "input_border_radius": "md",
    "input_padding": "",
    "input_background_color": "",
    "input_text_color": "",
    "input_border_color": "var:border",
    "input_border_width": "",
    "input_border_style": "",
    "input_focus_border_color": "var:primary",
    "input_placeholder_color": ""
  },
  "settings": {
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Example Response (Legacy Inline Fields Mode)

```json
{
  "widget_type": "form",
  "uuid": "form-widget-456",
  "config": {
    "form_uuid": "form-widget-456",
    "fields": [
      {
        "code": "name",
        "type": "text",
        "label": "Name",
        "placeholder": "Your name",
        "required": true,
        "options": []
      },
      {
        "code": "email",
        "type": "email",
        "label": "Email",
        "placeholder": "your@email.com",
        "required": true,
        "options": []
      },
      {
        "code": "message",
        "type": "textarea",
        "label": "Message",
        "placeholder": "Your message...",
        "required": false,
        "options": []
      }
    ],
    "submit_text": "Submit",
    "success_message": "Thank you! Your message has been sent.",
    "error_message": "Something went wrong. Please try again.",
    "button_color": "#50a5f1",
    "email_to": "contact@example.com"
  }
}
```

## How It Works

The form widget works in two steps:

1. **Page render**: The widget provides the `form_code` and layout settings
2. **Form fetch**: The frontend fetches form definition (fields, validation) from the Forms API using the `form_code`

```
GET /v1/:workspace/:project/forms/:form_code
```

This returns the form fields, validation rules, success message, and optional `captcha_site_key`. See [Forms API](../forms.md) for details.

## Layout Settings

### Label Position

| Value | Description |
|-------|-------------|
| `top` | Labels above input fields |
| `side` | Labels beside input fields |

### Columns

| Value | Description |
|-------|-------------|
| `"1"` | Single column layout |
| `"2"` | Two column layout |

### Button Align

| Value | Description |
|-------|-------------|
| `left` | Button aligned left |
| `center` | Button centered |
| `right` | Button aligned right |

## Submission

Form submissions go through the Forms API:

```
POST /v1/:workspace/:project/forms/:form_uuid/submit
```

### Request Body

```json
{
  "data": {
    "name": "John Doe",
    "email": "john@example.com",
    "message": "Hello, I have a question."
  },
  "_captcha_token": "0.turnstile_token_here..."
}
```

See [Forms API](../forms.md) for full submission documentation including Cloudflare Turnstile CAPTCHA.

## Usage Example

```javascript
async function renderFormWidget(widget, language, api) {
  const { form_code, submit_text, button_align, label_position, columns } = widget.config;

  // Fetch form definition from Forms API
  const { data: form } = await api.getForm(form_code);
  if (!form) return '<p>Form not found</p>';

  const submitLabel = submit_text?.[language] || submit_text?.en || 'Submit';

  const fields = form.fields.map(field => {
    const label = field.label_translation?.[language] || field.label;
    const required = field.required ? 'required' : '';

    if (field.type === 'textarea') {
      return `
        <div class="form-group">
          <label>${label}</label>
          <textarea name="${field.code}" placeholder="${field.placeholder || ''}" ${required}></textarea>
        </div>
      `;
    }

    return `
      <div class="form-group">
        <label>${label}</label>
        <input type="${field.type}" name="${field.code}" placeholder="${field.placeholder || ''}" ${required} />
      </div>
    `;
  }).join('');

  return `
    <form data-form-uuid="${form.uuid}" class="lcms-form columns-${columns} labels-${label_position}">
      ${fields}
      <div style="text-align: ${button_align};">
        <button type="submit">${submitLabel}</button>
      </div>
    </form>
  `;
}
```
