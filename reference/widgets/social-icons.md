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
| `config` | object | Widget configuration |
| `config.items` | array | Array of social media links |
| `config.items[].platform` | string | Platform identifier |
| `config.items[].url` | string | Profile/page URL |
| `config.items[].icon` | string | Optional custom icon class |
| `config.size` | string | Icon size: `"sm"`, `"md"`, `"lg"` |
| `config.style` | string | Icon style: `"default"`, `"circle"`, `"square"` |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "social-icons",
  "uuid": "social-123",
  "config": {
    "items": [
      {
        "platform": "facebook",
        "url": "https://facebook.com/mycompany",
        "icon": null
      },
      {
        "platform": "twitter",
        "url": "https://twitter.com/mycompany",
        "icon": null
      },
      {
        "platform": "instagram",
        "url": "https://instagram.com/mycompany",
        "icon": null
      },
      {
        "platform": "linkedin",
        "url": "https://linkedin.com/company/mycompany",
        "icon": null
      }
    ],
    "size": "md",
    "style": "circle"
  },
  "settings": {
    "horizontalAlign": "center",
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
| `circle` | Icons in circular containers |
| `square` | Icons in square containers |

## Usage Example

```javascript
const platformIcons = {
  facebook: 'fab fa-facebook-f',
  twitter: 'fab fa-x-twitter',
  instagram: 'fab fa-instagram',
  linkedin: 'fab fa-linkedin-in',
  youtube: 'fab fa-youtube',
  tiktok: 'fab fa-tiktok',
  pinterest: 'fab fa-pinterest-p',
  github: 'fab fa-github'
};

function renderSocialIcons(widget) {
  const { items, size, style } = widget.config;

  if (!items || items.length === 0) return '';

  const icons = items.map(item => {
    const icon = item.icon || platformIcons[item.platform] || 'fas fa-link';

    return `
      <a href="${item.url}"
         class="social-icon social-${item.platform} social-style-${style}"
         target="_blank"
         rel="noopener noreferrer"
         aria-label="${item.platform}"
      >
        <i class="${icon}"></i>
      </a>
    `;
  }).join('');

  return `
    <div class="social-icons-widget social-size-${size} social-style-${style}">
      ${icons}
    </div>
  `;
}
```
