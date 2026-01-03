# Social Icons Widget

A set of social media icon links.

## Widget Type

```
social-icons
```

## Data Properties

| Property | Type | Global | Description |
|----------|------|--------|-------------|
| `items` | array | Yes | Array of social media links |
| `size` | string | Yes | Icon size: `sm`, `md`, `lg` |
| `style` | string | Yes | Icon style: `default`, `circle`, `square` |

### Item Structure

Each item in the `items` array contains:

| Property | Type | Description |
|----------|------|-------------|
| `platform` | string | Social platform identifier |
| `url` | string | Profile/page URL |

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

## Example Response

```json
{
  "widget_type": "social-icons",
  "uuid": "social-123",
  "data": {
    "items": [
      {
        "platform": "facebook",
        "url": "https://facebook.com/mycompany"
      },
      {
        "platform": "twitter",
        "url": "https://twitter.com/mycompany"
      },
      {
        "platform": "instagram",
        "url": "https://instagram.com/mycompany"
      },
      {
        "platform": "linkedin",
        "url": "https://linkedin.com/company/mycompany"
      },
      {
        "platform": "youtube",
        "url": "https://youtube.com/@mycompany"
      }
    ],
    "size": "md",
    "style": "circle"
  },
  "settings": {
    "textAlign": "center"
  }
}
```

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
// Platform icon mapping
const platformIcons = {
  facebook: 'fa-brands fa-facebook-f',
  twitter: 'fa-brands fa-x-twitter',
  instagram: 'fa-brands fa-instagram',
  linkedin: 'fa-brands fa-linkedin-in',
  youtube: 'fa-brands fa-youtube',
  tiktok: 'fa-brands fa-tiktok',
  pinterest: 'fa-brands fa-pinterest-p',
  github: 'fa-brands fa-github'
};

// Platform colors
const platformColors = {
  facebook: '#1877F2',
  twitter: '#000000',
  instagram: '#E4405F',
  linkedin: '#0A66C2',
  youtube: '#FF0000',
  tiktok: '#000000',
  pinterest: '#BD081C',
  github: '#181717'
};

// Render social icons widget
function renderSocialIcons(widget) {
  const { items, size, style } = widget.data;

  if (!items || items.length === 0) {
    return '';
  }

  const sizeClasses = { sm: '24px', md: '32px', lg: '48px' };
  const iconSize = sizeClasses[size] || sizeClasses.md;

  const icons = items.map(item => {
    const icon = platformIcons[item.platform] || 'fa-solid fa-link';
    const color = platformColors[item.platform] || '#333';

    return `
      <a href="${item.url}"
         class="social-icon social-${item.platform} social-style-${style}"
         target="_blank"
         rel="noopener noreferrer"
         aria-label="${item.platform}"
         style="
           width: ${iconSize};
           height: ${iconSize};
           ${style !== 'default' ? `background: ${color};` : `color: ${color};`}
         "
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

## CSS Example

```css
.social-icons-widget {
  display: flex;
  gap: 12px;
  justify-content: center;
  flex-wrap: wrap;
}

.social-icon {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  text-decoration: none;
  transition: all 0.3s ease;
}

/* Default Style */
.social-style-default {
  background: none;
}

.social-style-default:hover {
  opacity: 0.7;
}

/* Circle Style */
.social-style-circle {
  border-radius: 50%;
  color: white;
}

.social-style-circle:hover {
  transform: scale(1.1);
}

/* Square Style */
.social-style-square {
  border-radius: 4px;
  color: white;
}

.social-style-square:hover {
  transform: scale(1.1);
}

/* Sizes */
.social-size-sm .social-icon { font-size: 12px; }
.social-size-md .social-icon { font-size: 16px; }
.social-size-lg .social-icon { font-size: 24px; }
```

## Accessibility

Include proper aria-labels for screen readers:

```javascript
`<a href="${url}"
   aria-label="Follow us on ${platformName}"
   title="Follow us on ${platformName}"
   target="_blank"
   rel="noopener noreferrer"
>
```
