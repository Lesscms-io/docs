# CTA Box Widget

A call-to-action box with title, subtitle, and button.

## Widget Type

```
cta-box
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"cta-box"` |
| `uuid` | string | Unique widget identifier |
| `content` | object | Widget content |
| `content.title` | object | Multilingual title |
| `content.subtitle` | object | Multilingual subtitle |
| `content.button_text` | object | Multilingual button label |
| `config` | object | Widget configuration |
| `config.button_url` | string | Button link URL |
| `config.background_color` | string\|null | Box background color |
| `config.button_color` | string\|null | Button background color |
| `config.text_color` | string\|null | Text color (hex or `var:*`) |
| `config.alignment` | string | Text alignment: `"left"`, `"center"`, `"right"` |
| `config.padding_y` | number | Vertical padding in pixels |
| `config.padding_x` | number | Horizontal padding in pixels |
| `config.border_radius` | number | Border radius in pixels |
| `config.title_font_size` | number | Title font size in pixels |
| `config.subtitle_font_size` | number | Subtitle font size in pixels |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "cta-box",
  "uuid": "cta-box-123",
  "content": {
    "title": {
      "en": "Ready to get started?",
      "pl": "Gotowy, żeby zacząć?"
    },
    "subtitle": {
      "en": "Join thousands of satisfied customers today.",
      "pl": "Dołącz do tysięcy zadowolonych klientów."
    },
    "button_text": {
      "en": "Start Free Trial",
      "pl": "Rozpocznij za darmo"
    }
  },
  "config": {
    "button_url": "https://example.com/signup",
    "background_color": "#50a5f1",
    "button_color": "#ffffff",
    "text_color": "#ffffff",
    "alignment": "center",
    "padding_y": 32,
    "padding_x": 24,
    "border_radius": 8,
    "title_font_size": 28,
    "subtitle_font_size": 16
  }
}
```

## Usage Example

```javascript
function renderCtaBox(widget, language) {
  const { title, subtitle, button_text } = widget.content;
  const {
    button_url, background_color, button_color, text_color, alignment,
    padding_y, padding_x, border_radius, title_font_size, subtitle_font_size
  } = widget.config;

  const titleText = title?.[language] || title?.en || '';
  const subtitleText = subtitle?.[language] || subtitle?.en || '';
  const btnText = button_text?.[language] || button_text?.en || '';

  return `
    <div class="cta-box" style="
      background-color: ${background_color || '#50a5f1'};
      color: ${text_color || '#fff'};
      text-align: ${alignment || 'center'};
      padding: ${padding_y || 32}px ${padding_x || 24}px;
      border-radius: ${border_radius || 8}px;
    ">
      ${titleText ? `<h3>${titleText}</h3>` : ''}
      ${subtitleText ? `<p>${subtitleText}</p>` : ''}
      ${btnText && button_url ? `
        <a href="${button_url}" class="cta-button" style="
          background-color: ${button_color || '#fff'};
          color: #212529;
          padding: 10px 24px;
          border-radius: 4px;
          text-decoration: none;
          font-weight: 600;
        ">${btnText}</a>
      ` : ''}
    </div>
  `;
}
```
