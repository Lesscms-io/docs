# Form Widget

A form widget that renders a form defined in LessCMS Forms. The widget references a form by code and provides layout/style customization.

## Widget Type

```
form
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"form"` |
| `uuid` | string | Unique widget identifier |
| `config` | object | Widget configuration |
| `config.form_code` | string | Code of the form to render (references Forms API) |
| `config.submit_text` | object | Multilingual submit button text `{ "en": "Send", "pl": "Wyslij" }` |
| `config.button_style` | string | Submit button CSS style |
| `config.button_size` | string | Submit button size |
| `config.button_border_radius` | string | Button border radius |
| `config.button_padding` | string | Button padding |
| `config.button_icon` | string | Button icon class |
| `config.button_icon_position` | string | Icon position relative to text |
| `config.button_align` | string | Button alignment: `"left"`, `"center"`, `"right"` |
| `config.label_position` | string | Label position: `"top"`, `"side"` |
| `config.columns` | string | Form columns: `"1"`, `"2"` |
| `config.input_size` | string | Input size |
| `config.input_border_radius` | string | Input border radius |
| `config.input_padding` | string | Input padding |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "form",
  "uuid": "form-widget-123",
  "config": {
    "form_code": "contact",
    "submit_text": {
      "en": "Send Message",
      "pl": "Wyslij wiadomosc"
    },
    "button_style": "primary",
    "button_size": "md",
    "button_border_radius": "4px",
    "button_padding": "",
    "button_icon": "",
    "button_icon_position": "",
    "button_align": "left",
    "label_position": "top",
    "columns": "1",
    "input_size": "",
    "input_border_radius": "",
    "input_padding": ""
  },
  "settings": {
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
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
