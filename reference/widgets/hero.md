# Hero Widget

A full-width hero section with background image, title, subtitle, and optional call-to-action button.

## Widget Type

```
hero
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"hero"` |
| `uuid` | string | Unique widget identifier |
| `content` | object | Widget content |
| `content.title` | object | Multilingual title text |
| `content.subtitle` | object | Multilingual subtitle text |
| `content.background_url` | string | Background image URL |
| `content.button_text` | object | Multilingual CTA button text |
| `content.button_url` | string | CTA button URL |
| `config` | object | Widget configuration |
| `config.button_style` | string | Button style: `"primary"`, `"secondary"`, etc. |
| `config.button_size` | string | Button size: `"sm"`, `"md"`, `"lg"` |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "hero",
  "uuid": "hero-123",
  "content": {
    "title": {
      "en": "Welcome to Our Website",
      "pl": "Witamy na naszej stronie"
    },
    "subtitle": {
      "en": "We build amazing digital experiences.",
      "pl": "Tworzymy niesamowite cyfrowe doświadczenia."
    },
    "background_url": "https://cdn.example.com/hero-bg.jpg",
    "button_text": {
      "en": "Get Started",
      "pl": "Rozpocznij"
    },
    "button_url": "/contact"
  },
  "config": {
    "button_style": "primary",
    "button_size": "lg"
  },
  "settings": {
    "minHeight": 600,
    "horizontalAlign": "center",
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Usage Example

```javascript
function renderHero(widget, language) {
  const { title, subtitle, background_url, button_text, button_url } = widget.content;
  const { button_style, button_size } = widget.config;

  const titleText = title?.[language] || title?.en || '';
  const subtitleText = subtitle?.[language] || subtitle?.en || '';
  const buttonText = button_text?.[language] || button_text?.en || '';

  return `
    <section class="hero" style="
      background-image: url('${background_url || ''}');
      background-size: cover;
      background-position: center;
      min-height: 500px;
      display: flex;
      align-items: center;
      justify-content: center;
    ">
      <div class="hero-content" style="text-align: center; color: white;">
        ${titleText ? `<h1>${titleText}</h1>` : ''}
        ${subtitleText ? `<p class="hero-subtitle">${subtitleText}</p>` : ''}
        ${buttonText && button_url ? `
          <a href="${button_url}" class="btn btn-${button_style} btn-${button_size}">${buttonText}</a>
        ` : ''}
      </div>
    </section>
  `;
}
```
