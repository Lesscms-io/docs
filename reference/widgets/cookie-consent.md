# Cookie Consent Widget

A cookie consent banner that remembers user's choice via localStorage.

## Widget Type

```
cookie-consent
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"cookie-consent"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget properties |
| `widget.message` | object | Multilingual consent message (HTML) |
| `widget.accept_text` | object | Multilingual accept button text |
| `widget.show_decline` | boolean | Show a decline button |
| `widget.decline_text` | object | Multilingual decline button text |
| `widget.policy_url` | string\|null | URL to privacy policy page |
| `widget.policy_link_text` | object | Multilingual privacy policy link text |
| `widget.position` | string | Banner position: `"bottom"`, `"top"`, `"bottom-left"`, `"bottom-right"` |
| `widget.cookie_style` | string | Banner style: `"bar"`, `"box"`, `"minimal"` |
| `widget.days_to_expire` | number | Days until consent expires (default: 365) |
| `widget.background_color` | string\|null | Custom background color |
| `widget.text_color` | string\|null | Custom text color |
| `widget.accept_bg_color` | string\|null | Custom accept button background color |
| `widget.accept_text_color` | string\|null | Custom accept button text color |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "cookie-consent",
  "uuid": "cookie-consent-123",
  "widget": {
    "message": {
      "en": "<p>We use cookies to improve your experience. By continuing to use this site, you agree to our use of cookies.</p>",
      "pl": "<p>Korzystamy z plików cookie, aby poprawić Twoje doświadczenia. Kontynuując korzystanie ze strony, zgadzasz się na użycie plików cookie.</p>"
    },
    "accept_text": {
      "en": "Accept",
      "pl": "Akceptuję"
    },
    "show_decline": true,
    "decline_text": {
      "en": "Decline",
      "pl": "Odrzucam"
    },
    "policy_url": "/privacy-policy",
    "policy_link_text": {
      "en": "Privacy Policy",
      "pl": "Polityka prywatności"
    },
    "position": "bottom",
    "cookie_style": "bar",
    "days_to_expire": 365,
    "background_color": "#1a1a2e",
    "text_color": "#e0e0e0",
    "accept_bg_color": "#4f46e5",
    "accept_text_color": "#ffffff"
  },
  "settings": {}
}
```

## Position Values

| Value | Description |
|-------|-------------|
| `bottom` | Full-width bar at the bottom of the page |
| `top` | Full-width bar at the top of the page |
| `bottom-left` | Floating box in the bottom-left corner |
| `bottom-right` | Floating box in the bottom-right corner |

## Style Values

| Value | Description |
|-------|-------------|
| `bar` | Horizontal bar with text and buttons side by side |
| `box` | Stacked layout with text above buttons |
| `minimal` | Compact bar with smaller text |

## Usage Example

```javascript
function renderCookieConsent(widget, language) {
  const { message, accept_text, show_decline, decline_text, policy_url, policy_link_text, position, style } = widget.widget;

  const STORAGE_KEY = 'lcms-cookie-consent';

  // Check if already consented
  const stored = localStorage.getItem(STORAGE_KEY);
  if (stored) {
    const parsed = JSON.parse(stored);
    if (parsed.expires && new Date(parsed.expires) > new Date()) {
      return ''; // Already consented, don't show
    }
  }

  const msgText = message?.[language] || message?.en || '';
  const acceptBtnText = accept_text?.[language] || accept_text?.en || 'OK';
  const declineBtnText = decline_text?.[language] || decline_text?.en || '';
  const policyText = policy_link_text?.[language] || policy_link_text?.en || '';

  return `
    <div class="cookie-consent cookie-consent--${position} cookie-consent--${style}">
      <div class="cookie-consent__body">
        <div class="cookie-consent__message">${msgText}</div>
        ${policy_url && policyText ? `<a href="${policy_url}" target="_blank">${policyText}</a>` : ''}
      </div>
      <div class="cookie-consent__actions">
        <button class="cookie-consent__accept" onclick="acceptCookies()">${acceptBtnText}</button>
        ${show_decline && declineBtnText ? `<button class="cookie-consent__decline" onclick="declineCookies()">${declineBtnText}</button>` : ''}
      </div>
    </div>
  `;
}
```
