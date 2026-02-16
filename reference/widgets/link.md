# Link Widget

A simple link widget with text, optional icon, and configurable hover animation.

## Widget Type

```
link
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"link"` |
| `uuid` | string | Unique widget identifier |
| `config` | object | Widget configuration |
| `config.icon` | string | Icon class (FontAwesome), default `"fa-solid fa-arrow-right"` |
| `config.icon_position` | string | `"left"`, `"right"`, or `"none"` |
| `config.animation` | string | Hover animation: `"none"`, `"slide"`, `"fade"`, `"underline"` |
| `config.color` | string\|null | Custom link color (hex) |
| `config.target_blank` | boolean | Open in new tab |
| `content` | object | Widget content |
| `content.text` | object | Multilingual link text |
| `data` | object | Link data |
| `data.url` | string | Link URL |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "link",
  "uuid": "link-123",
  "config": {
    "icon": "fa-solid fa-arrow-right",
    "icon_position": "right",
    "animation": "slide",
    "color": "#50a5f1",
    "target_blank": false
  },
  "content": {
    "text": {
      "en": "Learn more",
      "pl": "Dowiedz się więcej"
    }
  },
  "data": {
    "url": "https://example.com/about"
  },
  "settings": {
    "horizontalAlign": "left",
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Icon Positions

| Value | Description |
|-------|-------------|
| `left` | Icon displayed before text |
| `right` | Icon displayed after text (default) |
| `none` | No icon displayed |

## Animation Types

| Value | Description |
|-------|-------------|
| `none` | No animation (default) |
| `slide` | Icon slides on hover (moves away from text) |
| `fade` | Icon fades in on hover |
| `underline` | Underline appears on hover |

## Multi-Item Support

The link widget supports displaying multiple links in a grid. When multiple items are configured, the API returns a multi-item structure:

```json
{
  "widget_type": "link",
  "uuid": "link-multi",
  "multi_item": true,
  "multi_columns": 2,
  "multi_gap": 16,
  "items": [
    {
      "widget_type": "link",
      "config": { "icon": "fa-solid fa-arrow-right", "icon_position": "right", "animation": "slide", "color": "#50a5f1", "target_blank": false },
      "content": { "text": { "en": "About Us" } },
      "data": { "url": "/about" }
    },
    {
      "widget_type": "link",
      "config": { "icon": "fa-solid fa-arrow-right", "icon_position": "right", "animation": "slide", "color": "#50a5f1", "target_blank": false },
      "content": { "text": { "en": "Contact" } },
      "data": { "url": "/contact" }
    }
  ],
  "settings": {}
}
```

Per-item fields (`text`, `url`, `target_blank`) are unique to each item. Shared fields (`icon`, `icon_position`, `animation`, `color`) are the same across all items.

## Usage Example

```javascript
function renderLink(widget, language) {
  const { icon, icon_position, animation, color, target_blank } = widget.config;
  const text = widget.content?.text?.[language] || widget.content?.text?.en || '';
  const url = widget.data?.url || '#';

  const target = target_blank ? ' target="_blank" rel="noopener noreferrer"' : '';
  const style = color ? ` style="color: ${color}"` : '';

  let iconHtml = '';
  if (icon_position !== 'none') {
    iconHtml = `<i class="${icon} link-icon link-icon--${icon_position}"></i>`;
  }

  const leftIcon = icon_position === 'left' ? iconHtml : '';
  const rightIcon = icon_position === 'right' ? iconHtml : '';

  return `
    <a href="${url}" class="link link--animation-${animation}"${style}${target}>
      ${leftIcon}
      <span class="link-text">${text}</span>
      ${rightIcon}
    </a>
  `;
}
```

## CSS Animation Examples

```css
/* Slide animation */
.link--animation-slide:hover .link-icon--right {
  transform: translateX(5px);
}

.link--animation-slide:hover .link-icon--left {
  transform: translateX(-5px);
}

/* Fade animation */
.link--animation-fade .link-icon {
  opacity: 0.6;
}

.link--animation-fade:hover .link-icon {
  opacity: 1;
}

/* Underline animation */
.link--animation-underline {
  position: relative;
}

.link--animation-underline::after {
  content: '';
  position: absolute;
  bottom: -2px;
  left: 0;
  width: 0;
  height: 2px;
  background-color: currentColor;
  transition: width 0.3s ease;
}

.link--animation-underline:hover::after {
  width: 100%;
}
```
