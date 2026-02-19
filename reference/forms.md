# Forms

Contact forms and data collection. Forms are defined in the CMS and can be embedded on pages via the Form widget.

## Endpoints

<span class="endpoint-badge get">GET</span> `/v1/:workspace_code/:project_code/forms`

<span class="endpoint-badge get">GET</span> `/v1/:workspace_code/:project_code/forms/:form_code`

<span class="endpoint-badge post">POST</span> `/v1/:workspace_code/:project_code/forms/:form_uuid/submit`

---

## List Active Forms

Get all active forms in a project.

```
GET /v1/:workspace_code/:project_code/forms
```

### Example Request

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace/project/forms
```

### Example Response

```json
{
  "data": [
    {
      "uuid": "550e8400-e29b-41d4-a716-446655440000",
      "name": "Contact Form",
      "name_translation": {
        "en": "Contact Form",
        "pl": "Formularz kontaktowy"
      },
      "code": "contact",
      "description": "Main contact form",
      "fields": [
        {
          "code": "name",
          "label": "Name",
          "label_translation": { "pl": "Imię" },
          "type": "text",
          "required": true,
          "placeholder": "Your name"
        },
        {
          "code": "email",
          "label": "Email",
          "label_translation": { "pl": "Email" },
          "type": "email",
          "required": true,
          "placeholder": "your@email.com"
        },
        {
          "code": "message",
          "label": "Message",
          "label_translation": { "pl": "Wiadomość" },
          "type": "textarea",
          "required": true,
          "placeholder": "Your message..."
        }
      ],
      "settings": {
        "success_message": "Thank you! We'll get back to you soon.",
        "success_message_translation": {
          "pl": "Dziękujemy! Odezwiemy się wkrótce."
        }
      },
      "captcha_site_key": "0x4AAAAAAA..."
    }
  ]
}
```

### Response Headers

```
Cache-Tag: project-{project_uuid} project-{project_uuid}-forms
```

---

## Get Form by Code

Retrieve a specific form by its code.

```
GET /v1/:workspace_code/:project_code/forms/:form_code
```

### Path Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `form_code` | string | **Required.** Form code (e.g., `contact`, `newsletter`) |

### Example Request

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  https://api.lesscms.com/v1/workspace/project/forms/contact
```

### Example Response

```json
{
  "data": {
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "name": "Contact Form",
    "name_translation": {
      "en": "Contact Form",
      "pl": "Formularz kontaktowy"
    },
    "code": "contact",
    "description": "Main contact form",
    "fields": [
      {
        "code": "name",
        "label": "Name",
        "label_translation": { "pl": "Imię" },
        "type": "text",
        "required": true,
        "placeholder": "Your name"
      },
      {
        "code": "email",
        "label": "Email",
        "label_translation": { "pl": "Email" },
        "type": "email",
        "required": true,
        "placeholder": "your@email.com"
      },
      {
        "code": "message",
        "label": "Message",
        "label_translation": { "pl": "Wiadomość" },
        "type": "textarea",
        "required": true,
        "placeholder": "Your message..."
      }
    ],
    "settings": {
      "success_message": "Thank you! We'll get back to you soon.",
      "success_message_translation": {
        "pl": "Dziękujemy! Odezwiemy się wkrótce."
      }
    },
    "captcha_site_key": "0x4AAAAAAA..."
  }
}
```

> **Note:** The `captcha_site_key` field is only included when Cloudflare Turnstile is configured on the server. See [Cloudflare Turnstile](#cloudflare-turnstile) section below.

### Error Response

If form not found:

```json
{
  "error": "Form not found"
}
```

HTTP Status: `404 Not Found`

---

## Submit Form

Submit form data. Includes built-in anti-spam protection.

```
POST /v1/:workspace_code/:project_code/forms/:form_uuid/submit
```

### Path Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `form_uuid` | string | **Required.** Form UUID |

### Request Body

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `data` | object | Yes | Key-value pairs matching form field codes |
| `page_uuid` | string | No | UUID of the page the form was submitted from |
| `_captcha_token` | string | No* | Cloudflare Turnstile token. Required when Turnstile is configured on the server |

### Example Request

```bash
curl -X POST \
  -H "x-api-key: YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "data": {
      "name": "John Doe",
      "email": "john@example.com",
      "message": "Hello, I have a question."
    },
    "_captcha_token": "0.turnstile_token_here..."
  }' \
  https://api.lesscms.com/v1/workspace/project/forms/550e8400-e29b-41d4-a716-446655440000/submit
```

### Success Response

```json
{
  "success": true,
  "uuid": "660e8400-e29b-41d4-a716-446655440001"
}
```

HTTP Status: `201 Created`

### Validation Error

If a required field is missing:

```json
{
  "error": "Validation failed",
  "message": "Field \"Email\" is required",
  "field": "email"
}
```

HTTP Status: `422 Unprocessable Entity`

### Rate Limiting

Form submissions are rate-limited to **5 requests per IP per form per minute**. Exceeding the limit returns:

```json
{
  "error": "Too many submissions. Please try again later."
}
```

HTTP Status: `429 Too Many Requests`

---

## Anti-Spam Protection

Form submissions are protected by built-in proprietary anti-spam mechanisms that work automatically. No additional configuration is required — the protection is always active.

### Cloudflare Turnstile

When configured, LessCMS additionally uses [Cloudflare Turnstile](https://developers.cloudflare.com/turnstile/) for invisible CAPTCHA protection. The `captcha_site_key` field is included in form GET responses when Turnstile is active.

**Integration:**

1. Fetch the form via `GET /forms/:form_code` — check if `captcha_site_key` is present
2. If present, load the Turnstile SDK: `https://challenges.cloudflare.com/turnstile/v0/api.js`
3. Render the widget and send the generated token as `_captcha_token` in the submission

**Error response** (missing or invalid token):

```json
{
  "error": "CAPTCHA verification failed"
}
```

HTTP Status: `403 Forbidden`

> **Note:** If `captcha_site_key` is not present in the form response, Turnstile is not configured and `_captcha_token` is not required.

---

## Use Cases

### Render Dynamic Form

```javascript
async function renderForm(formCode, language) {
  const response = await fetch(
    `https://api.lesscms.com/v1/workspace/project/forms/${formCode}`,
    { headers: { 'x-api-key': API_KEY } }
  );

  const { data: form } = await response.json();

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
    <form data-form-uuid="${form.uuid}">
      ${fields}
      <button type="submit">Submit</button>
    </form>
  `;
}
```

### Submit with Validation

```javascript
async function handleSubmit(formElement) {
  const formData = {};
  const fields = formElement.querySelectorAll('input, textarea, select');

  fields.forEach(field => {
    if (field.name && !field.name.startsWith('_')) {
      formData[field.name] = field.value;
    }
  });

  const formUuid = formElement.dataset.formUuid;

  const response = await fetch(
    `https://api.lesscms.com/v1/workspace/project/forms/${formUuid}/submit`,
    {
      method: 'POST',
      headers: {
        'x-api-key': API_KEY,
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        data: formData
      })
    }
  );

  if (response.status === 422) {
    const error = await response.json();
    alert(`Validation error: ${error.message}`);
    return;
  }

  if (response.status === 429) {
    alert('Too many submissions. Please try again later.');
    return;
  }

  const result = await response.json();
  if (result.success) {
    alert('Form submitted successfully!');
  }
}
```

---

## Related Resources

- [Pages](pages.md)
- [Blocks](blocks.md)
- [Elements](elements.md)
