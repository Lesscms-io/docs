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
| `widget` | object | Widget properties |
| `widget.text` | object | Text element-group |
| `widget.text.html` | object | Multilingual link text (`{ "en": "...", "pl": "..." }`) |
| `widget.text.color` | string\|null | Text color (CSS or `"var:colorName"`) |
| `widget.text.color:hover` | string\|null | Text color on hover |
| `widget.icon` | object | Icon element-group |
| `widget.icon.icon` | string\|null | Font Awesome icon class |
| `widget.icon.position` | string | Icon position: `"left"`, `"right"`, or `"none"` |
| `widget.config` | object | Configuration |
| `widget.config.animation` | string | Hover animation: `"none"`, `"slide"`, `"fade"`, `"underline"` |
| `widget.config.target_blank` | boolean | Open in new tab |
| `widget.config.url` | string | Link URL |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "link",
  "uuid": "link-123",
  "widget": {},
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
      "widget": { "text": { "html": { "en": "About Us" }, "color": "#50a5f1", "color:hover": null }, "icon": { "icon": "fa-solid fa-arrow-right", "position": "right" }, "config": { "animation": "slide", "target_blank": false, "url": "/about" } }
    },
    {
      "widget_type": "link",
      "widget": { "text": { "html": { "en": "Contact" }, "color": "#50a5f1", "color:hover": null }, "icon": { "icon": "fa-solid fa-arrow-right", "position": "right" }, "config": { "animation": "slide", "target_blank": false, "url": "/contact" } }
    }
  ],
  "settings": {}
}
```

Per-item fields (`text.html`, `config.url`, `config.target_blank`) are unique to each item. Shared fields (`icon`, `text.color`, `config.animation`) are the same across all items.

## Usage Example

```javascript
function renderLink(widget, language) {
  const { text, icon, config } = widget.widget;
  const label = text?.html?.[language] || text?.html?.en || '';
  const url = config?.url || '#';
  const animation = config?.animation || 'none';
  const iconPosition = icon?.position || 'right';

  const target = config?.target_blank ? ' target="_blank" rel="noopener noreferrer"' : '';
  const style = text?.color ? ` style="color: ${text.color}"` : '';

  let iconHtml = '';
  if (iconPosition !== 'none' && icon?.icon) {
    iconHtml = `<i class="${icon.icon} link-icon link-icon--${iconPosition}"></i>`;
  }

  const leftIcon = iconPosition === 'left' ? iconHtml : '';
  const rightIcon = iconPosition === 'right' ? iconHtml : '';

  return `
    <a href="${url}" class="link link--animation-${animation}"${style}${target}>
      ${leftIcon}
      <span class="link-text">${label}</span>
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
