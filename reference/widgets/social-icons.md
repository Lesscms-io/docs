# Social Icons Widget

A set of social media icon links.

## Widget Type

```
social-icons
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"social-icons"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget data |
| `widget.items` | array | Array of social media links |
| `widget.size` | string | Icon size: `"sm"`, `"md"`, `"lg"` |
| `widget.icon_style` | string | Icon style: `"default"`, `"colored"`, `"outlined"`, `"circle"`, `"square"` |
| `widget.color_mode` | string | Color mode: `"brand"` (platform colors) or `"custom"` (single custom color) |
| `widget.icon_color` | string\|null | Custom color value when `color_mode` is `"custom"` |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "social-icons",
  "uuid": "social-123",
  "widget": {
    "items": [
      { "platform": "facebook", "url": "https://facebook.com/mycompany", "icon": null },
      { "platform": "twitter", "url": "https://twitter.com/mycompany", "icon": null },
      { "platform": "instagram", "url": "https://instagram.com/mycompany", "icon": null },
      { "platform": "linkedin", "url": "https://linkedin.com/company/mycompany", "icon": null }
    ],
    "size": "md",
    "icon_style": "circle",
    "color_mode": "brand",
    "icon_color": null
  },
  "settings": {
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Supported Platforms

| Platform | Icon |
|----------|------|
| `facebook` | Facebook |
| `twitter` | Twitter/X |
| `instagram` | Instagram |
| `linkedin` | LinkedIn |
| `youtube` | YouTube |
| `tiktok` | TikTok |
| `pinterest` | Pinterest |
| `github` | GitHub |

## Size Values

| Value | Description |
|-------|-------------|
| `sm` | Small (24px) |
| `md` | Medium (32px) |
| `lg` | Large (48px) |

## Style Values

| Value | Description |
|-------|-------------|
| `default` | Plain icons, no background |
| `colored` | Filled circular containers with platform/custom color |
| `outlined` | Circular border with platform/custom color, no fill |
| `circle` | Filled circular containers (alias for colored) |
| `square` | Filled square containers with rounded corners |

## Color Mode Values

| Value | Description |
|-------|-------------|
| `brand` | Each icon uses its platform brand color (default) |
| `custom` | All icons use the single color from `icon_color` |

## Usage Example

```javascript
function renderSocialIcons(widget) {
  const { items, size, icon_style, color_mode, icon_color } = widget.widget;

  if (!items || items.length === 0) return '';

  const icons = items.map(item => `
    <a href="${item.url}"
       class="social-icon social-style-${icon_style}"
       target="_blank"
       rel="noopener noreferrer"
       aria-label="${item.platform}"
    >
      <i class="${item.icon || platformIcons[item.platform] || 'fas fa-link'}"></i>
    </a>
  `).join('');

  return `
    <div class="social-icons-widget social-size-${size}">
      ${icons}
    </div>
  `;
}
```
