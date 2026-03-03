# Team Member Widget

A card displaying a team member with photo, name, position, bio and social media links.

## Widget Type

```
team-member
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"team-member"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget data |
| `widget.image` | string | Photo URL |
| `widget.style` | string | Card style: `"card"`, `"minimal"`, `"overlay"` |
| `widget.accent_color` | string | Accent color for position text and social icons |
| `widget.social_links` | array | Array of social link objects |
| `widget.social_links[].platform` | string | Platform: `"linkedin"`, `"twitter"`, `"facebook"`, `"instagram"`, `"github"`, `"email"` |
| `widget.social_links[].url` | string | Profile URL |
| `widget.name` | object | Multilingual name |
| `widget.position` | object | Multilingual position/title |
| `widget.bio` | object | Multilingual biography |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "team-member",
  "uuid": "member-123",
  "widget": {
    "image": "https://cdn.example.com/photos/john.jpg",
    "style": "card",
    "accent_color": "#50a5f1",
    "social_links": [
      { "platform": "linkedin", "url": "https://linkedin.com/in/john" },
      { "platform": "twitter", "url": "https://x.com/john" }
    ],
    "name": { "en": "John Smith", "pl": "Jan Kowalski" },
    "position": { "en": "CEO", "pl": "Dyrektor generalny" },
    "bio": { "en": "Leading the company since 2015.", "pl": "Prowadzi firmę od 2015 roku." }
  },
  "settings": {}
}
```

## Multi-Item Support

The team-member widget supports displaying multiple members in a grid. When multiple items are configured, the API returns a multi-item structure:

```json
{
  "widget_type": "team-member",
  "uuid": "member-multi",
  "multi_item": true,
  "multi_columns": 3,
  "multi_gap": 16,
  "items": [
    {
      "widget_type": "team-member",
      "widget": { "image": "https://cdn.example.com/john.jpg", "style": "card", "accent_color": "#50a5f1", "social_links": [{ "platform": "linkedin", "url": "https://linkedin.com/in/john" }], "name": { "en": "John Smith" }, "position": { "en": "CEO" }, "bio": { "en": "Company leader." } }
    },
    {
      "widget_type": "team-member",
      "widget": { "image": "https://cdn.example.com/jane.jpg", "style": "card", "accent_color": "#50a5f1", "social_links": [{ "platform": "linkedin", "url": "https://linkedin.com/in/jane" }], "name": { "en": "Jane Doe" }, "position": { "en": "CTO" }, "bio": { "en": "Tech lead." } }
    },
    {
      "widget_type": "team-member",
      "widget": { "image": "https://cdn.example.com/bob.jpg", "style": "card", "accent_color": "#50a5f1", "social_links": [{ "platform": "github", "url": "https://github.com/bob" }], "name": { "en": "Bob Wilson" }, "position": { "en": "Lead Developer" }, "bio": { "en": "Full-stack expert." } }
    }
  ],
  "settings": {}
}
```

Per-item fields (`image`, `name`, `position`, `bio`, `social_links`) are unique to each item. Shared fields (`style`, `accent_color`) are the same across all items.

## Card Styles

| Value | Description |
|-------|-------------|
| `card` | Standard card with shadow (default) |
| `minimal` | Minimal style with circular photo |
| `overlay` | Photo with text overlay at the bottom |

## Usage Example

```javascript
function renderTeamMember(widget, language) {
  const { image, style, accent_color, social_links } = widget.widget;
  const name = widget.widget?.name?.[language] || widget.widget?.name?.en || '';
  const position = widget.widget?.position?.[language] || widget.widget?.position?.en || '';
  const bio = widget.widget?.bio?.[language] || widget.widget?.bio?.en || '';

  const socialHtml = (social_links || []).map(link => {
    const icons = { linkedin: 'fab fa-linkedin', twitter: 'fab fa-x-twitter', facebook: 'fab fa-facebook', instagram: 'fab fa-instagram', github: 'fab fa-github', email: 'fas fa-envelope' };
    const url = link.platform === 'email' ? `mailto:${link.url}` : link.url;
    return `<a href="${url}" target="_blank" rel="noopener"><i class="${icons[link.platform] || 'fas fa-link'}"></i></a>`;
  }).join('');

  return `
    <div class="team-member team-member--${style}">
      ${image ? `<img src="${image}" alt="${name}" class="team-member__image" />` : ''}
      <div class="team-member__info">
        <h3>${name}</h3>
        <p class="team-member__position" style="color: ${accent_color || '#50a5f1'}">${position}</p>
        ${bio ? `<p class="team-member__bio">${bio}</p>` : ''}
        ${socialHtml ? `<div class="team-member__social">${socialHtml}</div>` : ''}
      </div>
    </div>
  `;
}
```
