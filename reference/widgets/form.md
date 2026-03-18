# Form Widget

A form widget that references a system form by code. The frontend fetches the form definition (fields, validation, consents) from the Forms API.

## Widget Type

```
form
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"form"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget data |
| `widget.form_code` | string | Code of the form to render (references Forms API) |
| `widget.submit_text` | string | Submit button text (multilingual) |
| `widget.button_style` | string | Submit button CSS style (default: `"info"`) |
| `widget.button_size` | string | Submit button size: `"sm"`, `"md"`, `"lg"` (default: `"md"`) |
| `widget.button_border_radius` | string | Button border radius: `"none"`, `"sm"`, `"md"`, `"lg"`, `"pill"` (default: `"md"`) |
| `widget.button_padding` | string | Button padding in px |
| `widget.button_icon` | string | Button icon class (e.g., `"fa-solid fa-paper-plane"`) |
| `widget.button_icon_position` | string | Icon position: `"left"` or `"right"` (default: `"left"`) |
| `widget.button_align` | string | Button alignment: `"left"`, `"center"`, `"right"` (default: `"left"`) |
| `widget.button_full_width` | boolean | Make submit button full width (default: `false`) |
| `widget.label_position` | string | Label position: `"top"`, `"side"` (default: `"top"`) |
| `widget.columns` | string | Form columns: `"1"`, `"2"` (default: `"1"`) |
| `widget.field_gap` | string | Gap between fields in px |
| `widget.input_size` | string | Input size: `"sm"`, `"md"`, `"lg"` (default: `"md"`) |
| `widget.input_border_radius` | string | Input border radius: `"none"`, `"sm"`, `"md"`, `"lg"`, `"pill"` (default: `"md"`) |
| `widget.input_padding` | string | Input padding in px |
| `widget.input_background_color` | string | Input background color (hex or `"var:name"`) |
| `widget.input_text_color` | string | Input text color |
| `widget.input_border_color` | string | Input border color |
| `widget.input_border_width` | string | Input border width in px |
| `widget.input_border_style` | string | Input border style: `"solid"`, `"dashed"`, `"dotted"` |
| `widget.input_focus_border_color` | string | Input border color on focus |
| `widget.input_placeholder_color` | string | Input placeholder text color |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "form",
  "uuid": "form-widget-123",
  "widget": {
    "form_code": "contact",
    "submit_text": "Send Message",
    "button_style": "info",
    "button_size": "md",
    "button_border_radius": "md",
    "button_padding": "",
    "button_icon": "",
    "button_icon_position": "left",
    "button_align": "left",
    "button_full_width": false,
    "label_position": "top",
    "columns": "1",
    "field_gap": "",
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

## How It Works

The form widget works in two steps:

1. **Page render**: The widget provides the `form_code` and layout/styling settings
2. **Form fetch**: The frontend fetches the form definition from the Forms API using the `form_code`

```
GET /v1/:workspace/:project/forms/:form_code
```

This returns the form fields, validation rules, consents, success message, and optional `captcha_site_key`.

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

## Usage Example

```javascript
async function renderFormWidget(widget, language, api) {
  const { form_code, submit_text, button_align, label_position, columns } = widget.widget;

  const { data: form } = await api.getForm(form_code);
  if (!form) return '<p>Form not found</p>';

  const submitLabel = typeof submit_text === 'object'
    ? (submit_text[language] || submit_text.en || 'Submit')
    : (submit_text || 'Submit');

  const fields = form.fields.map(field => {
    const label = field.label_translation?.[language] || field.label;
    return `
      <div class="form-group">
        <label>${label}</label>
        <input type="${field.type}" name="${field.code}" ${field.required ? 'required' : ''} />
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
