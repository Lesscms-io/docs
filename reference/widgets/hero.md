# Hero Widget

A full-width hero section with background image, title, subtitle, and optional call-to-action button.

## Widget Type

```
hero
```

## Data Properties

| Property | Type | Global | Description |
|----------|------|--------|-------------|
| `title` | object | No | Hero title text (multilingual) |
| `subtitle` | object | No | Hero subtitle/description (multilingual) |
| `background` | object | Yes | Background image data |
| `button_text` | object | No | CTA button text (multilingual) |
| `button_link` | string | Yes | CTA button URL |

### Background Object Structure

| Property | Type | Description |
|----------|------|-------------|
| `url` | string | Background image URL |

## Example Response

```json
{
  "widget_type": "hero",
  "uuid": "hero-123",
  "data": {
    "title": {
      "en": "Welcome to Our Website",
      "pl": "Witamy na naszej stronie"
    },
    "subtitle": {
      "en": "We build amazing digital experiences that help your business grow.",
      "pl": "Tworzymy niesamowite cyfrowe doświadczenia, które pomagają rozwijać Twój biznes."
    },
    "background": {
      "url": "https://cdn.example.com/hero-bg.jpg"
    },
    "button_text": {
      "en": "Get Started",
      "pl": "Rozpocznij"
    },
    "button_link": "/contact"
  },
  "settings": {
    "minHeight": 600,
    "textAlign": "center",
    "overlayColor": "#000000",
    "overlayOpacity": 40
  }
}
```

## Usage Example

```javascript
// Render hero widget
function renderHero(widget, language) {
  const { title, subtitle, background, button_text, button_link } = widget.data;
  const settings = widget.settings || {};

  const titleText = title?.[language] || title?.en || '';
  const subtitleText = subtitle?.[language] || subtitle?.en || '';
  const buttonText = button_text?.[language] || button_text?.en || '';

  const overlayColor = settings.overlayColor || '#000000';
  const overlayOpacity = (settings.overlayOpacity || 40) / 100;

  return `
    <section class="hero" style="
      position: relative;
      min-height: ${settings.minHeight || 500}px;
      background-image: url('${background?.url || ''}');
      background-size: cover;
      background-position: center;
      display: flex;
      align-items: center;
      justify-content: center;
      text-align: ${settings.textAlign || 'center'};
    ">
      <div class="hero-overlay" style="
        position: absolute;
        inset: 0;
        background: ${overlayColor};
        opacity: ${overlayOpacity};
      "></div>
      <div class="hero-content" style="
        position: relative;
        z-index: 1;
        color: white;
        padding: 40px 20px;
        max-width: 800px;
      ">
        ${titleText ? `<h1>${titleText}</h1>` : ''}
        ${subtitleText ? `<p class="hero-subtitle">${subtitleText}</p>` : ''}
        ${buttonText && button_link ? `
          <a href="${button_link}" class="hero-cta">${buttonText}</a>
        ` : ''}
      </div>
    </section>
  `;
}
```

## Styling Example

```css
.hero h1 {
  font-size: 48px;
  font-weight: 700;
  margin-bottom: 20px;
  text-shadow: 0 2px 4px rgba(0,0,0,0.3);
}

.hero-subtitle {
  font-size: 20px;
  margin-bottom: 30px;
  opacity: 0.9;
}

.hero-cta {
  display: inline-block;
  padding: 16px 32px;
  background: #007BFF;
  color: white;
  text-decoration: none;
  border-radius: 4px;
  font-weight: 600;
  transition: background 0.3s;
}

.hero-cta:hover {
  background: #0056b3;
}

/* Responsive */
@media (max-width: 768px) {
  .hero h1 {
    font-size: 32px;
  }

  .hero-subtitle {
    font-size: 16px;
  }
}
```
