# Form Widget

A contact form widget with dynamic fields, anti-spam protection, and email notifications.

## Widget Type

```
form
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"form"` |
| `uuid` | string | Unique widget identifier (also used as `form_uuid` for submissions) |
| `config` | object | Widget configuration |
| `config.fields` | array | List of form field definitions |
| `config.fields[].code` | string | Field identifier |
| `config.fields[].type` | string | Field type: `"text"`, `"email"`, `"textarea"`, `"select"`, `"checkbox"` |
| `config.fields[].label` | string/object | Field label (string or multilingual object) |
| `config.fields[].placeholder` | string/object | Placeholder text (string or multilingual object) |
| `config.fields[].required` | boolean | Whether the field is required |
| `config.fields[].options` | array | Options for `select` fields: `[{value, label}]` |
| `config.submit_text` | string/object | Submit button text (multilingual) |
| `config.success_message` | string/object | Message shown after successful submission (multilingual) |
| `config.error_message` | string/object | Message shown on submission error (multilingual) |
| `config.button_color` | string | Hex color for submit button (e.g. `"#50a5f1"`) |
| `config.form_uuid` | string | UUID used for the submission endpoint |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "form",
  "uuid": "form-abc-123",
  "config": {
    "form_uuid": "form-abc-123",
    "fields": [
      {
        "code": "name",
        "type": "text",
        "label": {
          "en": "Full Name",
          "pl": "Imie i nazwisko"
        },
        "placeholder": {
          "en": "Enter your name",
          "pl": "Wpisz swoje imie"
        },
        "required": true
      },
      {
        "code": "email",
        "type": "email",
        "label": {
          "en": "Email",
          "pl": "Email"
        },
        "placeholder": {
          "en": "your@email.com",
          "pl": "twoj@email.com"
        },
        "required": true
      },
      {
        "code": "message",
        "type": "textarea",
        "label": {
          "en": "Message",
          "pl": "Wiadomosc"
        },
        "placeholder": {
          "en": "Your message...",
          "pl": "Twoja wiadomosc..."
        },
        "required": true
      },
      {
        "code": "category",
        "type": "select",
        "label": "Category",
        "required": false,
        "options": [
          { "value": "general", "label": "General Inquiry" },
          { "value": "support", "label": "Support" },
          { "value": "sales", "label": "Sales" }
        ]
      },
      {
        "code": "agree_terms",
        "type": "checkbox",
        "label": {
          "en": "I agree to the terms",
          "pl": "Akceptuje regulamin"
        },
        "required": true
      }
    ],
    "submit_text": {
      "en": "Send Message",
      "pl": "Wyslij wiadomosc"
    },
    "success_message": {
      "en": "Thank you! Your message has been sent.",
      "pl": "Dziekujemy! Wiadomosc zostala wyslana."
    },
    "error_message": {
      "en": "Something went wrong. Please try again.",
      "pl": "Cos poszlo nie tak. Sprobuj ponownie."
    },
    "button_color": "#50a5f1"
  },
  "settings": {
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Submission Endpoint

Forms are submitted via POST to the public API:

```
POST /forms/{form_uuid}/submit
```

### Request Body

```json
{
  "data": {
    "name": "John Doe",
    "email": "john@example.com",
    "message": "Hello, I have a question."
  },
  "_hp_field": "",
  "_ts": 1708300000000
}
```

| Field | Type | Description |
|-------|------|-------------|
| `data` | object | Key-value pairs matching form field codes |
| `_hp_field` | string | Honeypot field (must be empty for valid submissions) |
| `_ts` | number | Timestamp when form was loaded (submissions < 2s are rejected) |

### Response (Success)

```json
{
  "success": true,
  "message": "Form submitted successfully"
}
```

### Response (Validation Error)

```json
{
  "error": "Missing required field: email"
}
```

### Rate Limiting

- **5 submissions per IP per form per minute**
- Returns `429 Too Many Requests` when exceeded

## Anti-Spam Protection

The form widget includes built-in anti-spam measures:

1. **Honeypot field** — A hidden input field (`_hp_field`). If filled (by bots), the submission returns 200 OK but is silently discarded.
2. **Timestamp validation** — The form records when it was loaded (`_ts`). Submissions made less than 2 seconds after loading are rejected as likely bot activity.
3. **Rate limiting** — Maximum 5 submissions per IP per form per minute.

## Email Notifications

When `email_to` is configured in form settings (via the CMS admin panel), each submission triggers an email notification to the specified address. The email contains:

- All submitted field values in a formatted table
- Submitter's IP address
- Submission date and time

## Field Types

| Type | HTML Element | Description |
|------|-------------|-------------|
| `text` | `<input type="text">` | Single-line text input |
| `email` | `<input type="email">` | Email input with validation |
| `textarea` | `<textarea>` | Multi-line text input |
| `select` | `<select>` | Dropdown with predefined options |
| `checkbox` | `<input type="checkbox">` | Boolean checkbox |

## Implementation Notes

- Labels and placeholders can be either plain strings or multilingual objects (`{ "en": "...", "pl": "..." }`)
- The Vue renderer (`LcmsForm.vue`) handles both formats via `extractValue()`
- Client-side validation runs before submission (required fields, email format)
- The form hides after successful submission and shows the success message
- Button color is applied as inline CSS (`backgroundColor`, `borderColor`)
