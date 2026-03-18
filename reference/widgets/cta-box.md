# CTA Box Widget

A call-to-action box with title, subtitle, and button. Uses nested element-group structure. Wrappable for multi-column layouts.

## Widget Type

```
cta-box
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"cta-box"` |
| `uuid` | string | Unique widget identifier |
| `widget.heading` | object | Heading element |
| `widget.heading.text` | object | Multilingual heading text |
| `widget.heading.color` | string\|null | Heading text color |
| `widget.heading.color:hover` | string\|null | Heading text color on hover |
| `widget.subtitle` | object | Subtitle element |
| `widget.subtitle.text` | object | Multilingual subtitle text |
| `widget.button` | object | Button element |
| `widget.button.text` | object | Multilingual button label |
| `widget.button.style` | string | Button style name (e.g. `"white"`, `"primary"`, `"outline-primary"`) |
| `widget.button.size` | string | Button size: `"sm"`, `"md"`, `"lg"` (default: `"md"`) |
| `widget.button.border_radius` | string | Button border radius: `"none"`, `"sm"`, `"md"`, `"lg"`, `"pill"` (default: `"md"`) |
| `widget.button.padding` | string | Custom button padding in px |
| `widget.button.icon` | string | Button icon class (e.g. `"fa-solid fa-arrow-right"`) |
| `widget.button.icon_position` | string | Icon position: `"left"` or `"right"` (default: `"left"`) |
| `widget.button.color` | string\|null | Button color (var:* or hex) |
| `widget.config` | object | Configuration element |
| `widget.config.alignment` | string | Text alignment: `"left"`, `"center"`, `"right"` (default: `"center"`) |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Wrappable

Yes — can be wrapped in a multi-column grid layout.

## Example Response

```json
{
  "widget_type": "cta-box",
  "uuid": "cta-box-123",
  "widget": {
    "heading": {
      "text": {
        "en": "Ready to get started?",
        "pl": "Zacznij już teraz"
      },
      "color": "#ffffff",
      "color:hover": null
    },
    "subtitle": {
      "text": {
        "en": "Join thousands of satisfied customers today.",
        "pl": "Dołącz do nas i odkryj nowe możliwości"
      }
    },
    "button": {
      "text": {
        "en": "Start Free Trial",
        "pl": "Sprawdź"
      },
      "style": "white",
      "size": "md",
      "border_radius": "md",
      "padding": "",
      "icon": "",
      "icon_position": "left",
      "color": "var:white"
    },
    "config": {
      "alignment": "center"
    }
  },
  "settings": {
    "padding_top": 20,
    "padding_right": 20,
    "padding_bottom": 20,
    "padding_left": 20,
    "background_color": "var:primary",
    "border_radius": 8,
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Usage Example

```javascript
function renderCtaBox(widget, language) {
  const { heading, subtitle, button, config } = widget.widget;

  const titleText = heading.text?.[language] || heading.text?.en || '';
  const subtitleText = subtitle.text?.[language] || subtitle.text?.en || '';
  const btnText = button.text?.[language] || button.text?.en || '';
  const alignment = config.alignment || 'center';

  return `
    <div class="cta-box" style="text-align: ${alignment};">
      ${titleText ? `<h3 style="color: ${heading.color || 'inherit'}">${titleText}</h3>` : ''}
      ${subtitleText ? `<p>${subtitleText}</p>` : ''}
      ${btnText ? `
        <a class="cta-button" style="
          border-radius: ${{ none: '0', sm: '4px', md: '8px', lg: '12px', pill: '50px' }[button.border_radius] || '8px'};
          ${button.padding ? `padding: ${button.padding}px;` : ''}
        ">
          ${button.icon && button.icon_position === 'left' ? `<i class="${button.icon}"></i> ` : ''}
          ${btnText}
          ${button.icon && button.icon_position === 'right' ? ` <i class="${button.icon}"></i>` : ''}
        </a>
      ` : ''}
    </div>
  `;
}
```
