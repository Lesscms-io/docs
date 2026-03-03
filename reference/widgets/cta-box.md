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
| `config.button_link_type` | string | Link type: `"url"`, `"page"`, `"collection"` (default: `"url"`) |
| `config.button_page_id` | string | Page ID for internal page link (when `button_link_type` is `"page"`) |
| `config.button_collection_code` | string | Collection code for collection link (when `button_link_type` is `"collection"`) |
| `config.button_entry_id` | string | Collection entry ID for entry link (when `button_link_type` is `"collection"`) |
| `config.button_route_uuid` | string\|null | Route UUID for link URL resolution (default: `null`) |
| `config.button_target_blank` | boolean | Open button link in new tab (default: `false`) |
| `config.background_color` | string\|null | Box background color |
| `config.button_color` | string\|null | Button background color |
| `config.button_style` | string | Button CSS style class (e.g. `"info"`, `"success"`) |
| `config.button_size` | string | Button size: `"sm"`, `"md"`, `"lg"` (default: `"md"`) |
| `config.button_border_radius` | string | Button border radius: `"none"`, `"sm"`, `"md"`, `"lg"`, `"pill"` (default: `"md"`) |
| `config.button_padding` | string | Custom button padding in px |
| `config.button_icon` | string | Button icon class (e.g. `"fa-solid fa-arrow-right"`) |
| `config.button_icon_position` | string | Icon position: `"left"` or `"right"` (default: `"left"`) |
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
    "button_link_type": "url",
    "button_page_id": "",
    "button_collection_code": "",
    "button_entry_id": "",
    "background_color": "#50a5f1",
    "button_color": "#ffffff",
    "button_style": "",
    "button_size": "md",
    "button_border_radius": "md",
    "button_padding": "",
    "button_icon": "",
    "button_icon_position": "left",
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
