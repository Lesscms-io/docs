# Hero Widget

A full-width hero section with background image, title, subtitle, and optional call-to-action button. Uses nested element-group structure. Supports both static content and dynamic content from collection entries.

## Widget Type

```
hero
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"hero"` |
| `uuid` | string | Unique widget identifier |
| `widget.heading` | object | Heading element group |
| `widget.heading.title` | object | Multilingual title text `{ "en": "...", "pl": "..." }` |
| `widget.heading.subtitle` | object | Multilingual subtitle text |
| `widget.button` | object | Button element group |
| `widget.button.text` | object | Multilingual CTA button text |
| `widget.button.url` | string | CTA button URL (default: `"#"`) |
| `widget.button.style` | string | Button style variant (default: `"primary"`) |
| `widget.button.size` | string | Button size: `"sm"`, `"md"`, `"lg"` (default: `"md"`) |
| `widget.button.border_radius` | string | Button border radius preset: `"none"`, `"sm"`, `"md"`, `"lg"`, `"full"` (default: `"md"`) |
| `widget.button.padding` | string | Custom button padding CSS value (default: `""`) |
| `widget.button.icon` | string | Button icon class (Font Awesome) (default: `""`) |
| `widget.button.icon_position` | string | Button icon position: `"left"`, `"right"` (default: `"left"`) |
| `widget.button.color` | string\|null | Custom button color (default: `null`) |
| `widget.button.link_type` | string | Button link type: `"custom"`, `"page"`, `"entry"` (default: `"custom"`) |
| `widget.button.page_id` | string\|null | Page UUID for page link type (default: `null`) |
| `widget.button.entry_id` | string\|null | Entry UUID for entry link type (default: `null`) |
| `widget.button.collection_code` | string\|null | Collection code for entry link type (default: `null`) |
| `widget.button.route_uuid` | string\|null | Route UUID for link URL resolution (default: `null`) |
| `widget.button.target_blank` | boolean | Open button link in new tab (default: `false`) |
| `widget.config` | object | Configuration element group |
| `widget.config.content_source` | string | Content source: `"static"` or `"dynamic"` (default: `"static"`) |
| `widget.config.text_align` | string | Text alignment: `"left"`, `"center"`, `"right"` (default: `"center"`) |
| `widget.config.text_position` | string | Vertical text position: `"top"`, `"center"`, `"bottom"` (default: `"center"`) |
| `widget.config.collection_code` | string | Collection code for dynamic mode (default: `""`) |
| `widget.config.entry_source` | string | Entry source: `"static"` or `"url"` (default: `"static"`) |
| `widget.config.entry_id` | string | Specific entry ID for dynamic mode (default: `""`) |
| `widget.config.entry_url_segment` | number | URL segment index for entry (default: `1`) |
| `widget.config.field_code_title` | string | Field code for dynamic title (default: `""`) |
| `widget.config.field_code_subtitle` | string | Field code for dynamic subtitle (default: `""`) |
| `widget.config.field_code_image` | string | Field code for dynamic background image (default: `""`) |
| `widget.config.show_title` | boolean | Show title (default: `true`) |
| `widget.config.show_subtitle` | boolean | Show subtitle (default: `false`) |
| `widget.config.background` | string\|null | Background image URL (default: `null`) |
| `widget.text` | object | Text styling element group |
| `widget.text.color` | string | Text color (default: `"#ffffff"`) |
| `widget.text.color:hover` | string\|null | Text color on hover (default: `null`) |
| `widget.overlay` | object | Overlay element group |
| `widget.overlay.color` | string | Overlay color (default: `"#000000"`) |
| `widget.overlay.opacity` | number | Overlay opacity 0-1 (default: `0.4`) |
| `widget.overlay.color:hover` | string\|null | Overlay color on hover (default: `null`) |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "hero",
  "uuid": "hero-123",
  "widget": {
    "heading": {
      "title": {
        "en": "Welcome to Our Website",
        "pl": "Witamy na naszej stronie"
      },
      "subtitle": {
        "en": "We build amazing digital experiences.",
        "pl": "Tworzymy niesamowite cyfrowe doswiadczenia."
      }
    },
    "button": {
      "text": {
        "en": "Get Started",
        "pl": "Rozpocznij"
      },
      "url": "/contact",
      "style": "primary",
      "size": "md",
      "border_radius": "md",
      "padding": "",
      "icon": "",
      "icon_position": "left",
      "color": null,
      "link_type": "custom",
      "page_id": null,
      "entry_id": null,
      "collection_code": null,
      "route_uuid": null,
      "target_blank": false
    },
    "config": {
      "content_source": "static",
      "text_align": "center",
      "text_position": "center",
      "collection_code": "",
      "entry_source": "static",
      "entry_id": "",
      "entry_url_segment": 1,
      "field_code_title": "",
      "field_code_subtitle": "",
      "field_code_image": "",
      "show_title": true,
      "show_subtitle": false,
      "background": "https://cdn.example.com/hero-bg.jpg"
    },
    "text": {
      "color": "#ffffff",
      "color:hover": null
    },
    "overlay": {
      "color": "#000000",
      "opacity": 0.4,
      "color:hover": null
    }
  },
  "settings": {
    "padding_top": 40,
    "padding_right": 20,
    "padding_bottom": 40,
    "padding_left": 20,
    "transition_duration": 200,
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Content Source Values

| Value | Description |
|-------|-------------|
| `static` | Use static content from `widget.heading` and `widget.config.background` |
| `dynamic` | Fetch content from collection entry using field mappings |

## Entry Source Values (Dynamic Mode)

| Value | Description |
|-------|-------------|
| `static` | Use specific `config.entry_id` |
| `url` | Extract entry ID from URL segment at `config.entry_url_segment` index |

## Text Align Values

| Value | Description |
|-------|-------------|
| `left` | Left-aligned text |
| `center` | Center-aligned text |
| `right` | Right-aligned text |

## Text Position Values

| Value | Description |
|-------|-------------|
| `top` | Content positioned at top |
| `center` | Content centered vertically |
| `bottom` | Content positioned at bottom |

## Usage Example

```javascript
function renderHero(widget, language) {
  const { heading, button, config, text, overlay } = widget.widget;

  const title = heading.title?.[language] || heading.title?.en || '';
  const subtitle = heading.subtitle?.[language] || heading.subtitle?.en || '';
  const buttonText = button.text?.[language] || button.text?.en || '';
  const backgroundUrl = config.background;

  const alignItems = config.text_position === 'top' ? 'flex-start'
    : config.text_position === 'bottom' ? 'flex-end' : 'center';

  return `
    <section class="hero" style="
      position: relative;
      min-height: 500px;
      display: flex;
      align-items: ${alignItems};
      justify-content: center;
    ">
      <div class="hero-bg" style="
        position: absolute;
        inset: 0;
        background-image: url('${backgroundUrl || ''}');
        background-size: cover;
        background-position: center;
      "></div>
      <div class="hero-overlay" style="
        position: absolute;
        inset: 0;
        background-color: ${overlay.color};
        opacity: ${overlay.opacity};
      "></div>
      <div class="hero-content" style="
        position: relative;
        z-index: 1;
        text-align: ${config.text_align};
        color: ${text.color};
        padding: 2rem;
      ">
        ${title ? `<h1>${title}</h1>` : ''}
        ${subtitle ? `<p class="hero-subtitle">${subtitle}</p>` : ''}
        ${buttonText && button.url ? `
          <a href="${button.url}"
             class="btn btn-${button.style} btn-${button.size}"
             ${button.target_blank ? 'target="_blank" rel="noopener noreferrer"' : ''}>
            ${button.icon && button.icon_position === 'left' ? `<i class="${button.icon}"></i> ` : ''}
            ${buttonText}
            ${button.icon && button.icon_position === 'right' ? ` <i class="${button.icon}"></i>` : ''}
          </a>
        ` : ''}
      </div>
    </section>
  `;
}
```
