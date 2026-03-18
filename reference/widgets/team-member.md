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
| `widget.member` | object | Member element group |
| `widget.member.name` | object | Multilingual member name |
| `widget.member.position` | object | Multilingual position/title |
| `widget.member.bio` | object | Multilingual biography |
| `widget.image` | object | Image element group |
| `widget.image.image` | string\|null | Photo URL |
| `widget.social` | object | Social element group |
| `widget.social.social_links` | array | Array of social link objects with `platform` (`"linkedin"`, `"twitter"`, `"facebook"`, `"instagram"`, `"github"`, `"email"`) and `url` properties |
| `widget.config` | object | Configuration group |
| `widget.config.team_member_style` | string | Card style: `"card"`, `"minimal"`, `"overlay"` |
| `widget.config.accent_color` | string\|null | Accent color for position text and social icons |
| `widget.config.accent_color:hover` | string\|null | Accent color on hover |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "team-member",
  "uuid": "member-123",
  "widget": {
    "member": {
      "name": { "en": "John Smith", "pl": "Jan Kowalski" },
      "position": { "en": "CEO", "pl": "Dyrektor generalny" },
      "bio": { "en": "Leading the company since 2015.", "pl": "Prowadzi firme od 2015 roku." }
    },
    "image": {
      "image": "https://cdn.example.com/photos/john.jpg"
    },
    "social": {
      "social_links": [
        { "platform": "linkedin", "url": "https://linkedin.com/in/john" },
        { "platform": "twitter", "url": "https://x.com/john" }
      ]
    },
    "config": {
      "team_member_style": "card",
      "accent_color": "var:primary",
      "accent_color:hover": null
    }
  },
  "settings": {}
}
```

## Card Styles

| Value | Description |
|-------|-------------|
| `card` | Standard card with shadow (default) |
| `minimal` | Minimal style with circular photo |
| `overlay` | Photo with text overlay at the bottom |

## Usage Example

```javascript
function renderTeamMember(widget, language) {
  const { member, image, social, config } = widget.widget;

  const name = member?.name?.[language] || member?.name?.en || '';
  const position = member?.position?.[language] || member?.position?.en || '';
  const bio = member?.bio?.[language] || member?.bio?.en || '';
  const photo = image?.image || null;
  const style = config?.team_member_style || 'card';
  const accentColor = config?.accent_color || '#50a5f1';
  const socialLinks = social?.social_links || [];

  const socialHtml = socialLinks.map(link => {
    const icons = { linkedin: 'fab fa-linkedin', twitter: 'fab fa-x-twitter', facebook: 'fab fa-facebook', instagram: 'fab fa-instagram', github: 'fab fa-github', email: 'fas fa-envelope' };
    const url = link.platform === 'email' ? `mailto:${link.url}` : link.url;
    return `<a href="${url}" target="_blank" rel="noopener"><i class="${icons[link.platform] || 'fas fa-link'}"></i></a>`;
  }).join('');

  return `
    <div class="team-member team-member--${style}">
      ${photo ? `<img src="${photo}" alt="${name}" class="team-member__image" />` : ''}
      <div class="team-member__info">
        <h3>${name}</h3>
        <p class="team-member__position" style="color: ${accentColor}">${position}</p>
        ${bio ? `<p class="team-member__bio">${bio}</p>` : ''}
        ${socialHtml ? `<div class="team-member__social">${socialHtml}</div>` : ''}
      </div>
    </div>
  `;
}
```
