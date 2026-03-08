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
| `widget` | object | Widget data |
| `widget.title` | object | Multilingual title |
| `widget.subtitle` | object | Multilingual subtitle |
| `widget.button_text` | object | Multilingual button label |
| `widget.button_url` | string | Button link URL |
| `widget.button_link_type` | string | Link type: `"url"`, `"page"`, `"collection"` (default: `"url"`) |
| `widget.button_page_id` | string | Page ID for internal page link (when `button_link_type` is `"page"`) |
| `widget.button_collection_code` | string | Collection code for collection link (when `button_link_type` is `"collection"`) |
| `widget.button_entry_id` | string | Collection entry ID for entry link (when `button_link_type` is `"collection"`) |
| `widget.button_route_uuid` | string\|null | Route UUID for link URL resolution (default: `null`) |
| `widget.button_target_blank` | boolean | Open button link in new tab (default: `false`) |
| `widget.background_color` | string\|null | Box background color |
| `widget.button_color` | string\|null | Button background color |
| `widget.button_style` | string | Button CSS style class (e.g. `"info"`, `"success"`) |
| `widget.button_size` | string | Button size: `"sm"`, `"md"`, `"lg"` (default: `"md"`) |
| `widget.button_border_radius` | string | Button border radius: `"none"`, `"sm"`, `"md"`, `"lg"`, `"pill"` (default: `"md"`) |
| `widget.button_padding` | string | Custom button padding in px |
| `widget.button_icon` | string | Button icon class (e.g. `"fa-solid fa-arrow-right"`) |
| `widget.button_icon_position` | string | Icon position: `"left"` or `"right"` (default: `"left"`) |
| `widget.text_color` | string\|null | Text color (hex or `var:*`) |
| `widget.alignment` | string | Text alignment: `"left"`, `"center"`, `"right"` |
| `widget.padding_y` | number | Vertical padding in pixels |
| `widget.padding_x` | number | Horizontal padding in pixels |
| `widget.border_radius` | number | Border radius in pixels |
| `widget.title_font_size` | number | Title font size in pixels |
| `widget.subtitle_font_size` | number | Subtitle font size in pixels |
| `widget.hover_background_color` | string\|null | Background color on hover |
| `widget.hover_text_color` | string\|null | Text color on hover |
| `widget.hover_lift` | number | Hover lift in pixels — translateY offset (default: `0`) |
| `widget.hover_scale` | number | Hover scale factor (default: `1`) |
| `widget.hover_shadow` | string | Hover shadow preset: `"none"`, `"sm"`, `"md"`, `"lg"` (default: `"none"`) |
| `widget.transition_duration` | number | Hover transition duration in ms (default: 200) |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "cta-box",
  "uuid": "cta-box-123",
  "widget": {
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
    },
    "button_url": "https://example.com/signup",
    "button_link_type": "url",
    "button_page_id": "",
    "button_collection_code": "",
    "button_entry_id": "",
    "button_route_uuid": null,
    "button_target_blank": false,
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
    "subtitle_font_size": 16,
    "hover_background_color": null,
    "hover_text_color": null,
    "hover_lift": 0,
    "hover_scale": 1,
    "hover_shadow": "none",
    "transition_duration": 200
  }
}
```

## Usage Example

```javascript
function renderCtaBox(widget, language) {
  const { title, subtitle, button_text, button_url, background_color, button_color, text_color, alignment,
    padding_y, padding_x, border_radius, title_font_size, subtitle_font_size } = widget.widget;

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
