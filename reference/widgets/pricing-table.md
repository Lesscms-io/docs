# Pricing Table Widget

A pricing card with title, price, feature list and call-to-action button. Uses nested element-group structure. Ideal for displaying subscription plans or service tiers.

## Widget Type

```
pricing-table
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"pricing-table"` |
| `uuid` | string | Unique widget identifier |
| `widget.heading` | object | Heading element group |
| `widget.heading.title` | object | Multilingual plan name |
| `widget.heading.subtitle` | object | Multilingual plan description |
| `widget.heading.price` | object | Multilingual price display (e.g., "$29") |
| `widget.heading.period` | object | Multilingual billing period (e.g., "month") |
| `widget.badge` | object | Badge element group |
| `widget.badge.badge` | object | Multilingual badge text (e.g., "Most Popular") |
| `widget.button` | object | Button element group |
| `widget.button.text` | object | Multilingual CTA button text |
| `widget.button.style` | string | Button style name (Bootstrap variant) (default: `"primary"`) |
| `widget.button.size` | string | Button size: `"sm"`, `"md"`, `"lg"` (default: `"md"`) |
| `widget.button.border_radius` | string | Button border radius preset: `"none"`, `"sm"`, `"md"`, `"lg"`, `"full"` (default: `"md"`) |
| `widget.button.padding` | string | Custom button padding CSS value (default: `""`) |
| `widget.button.icon` | string | Button icon class (Font Awesome) (default: `""`) |
| `widget.button.icon_position` | string | Button icon position: `"left"`, `"right"` (default: `"left"`) |
| `widget.button.color` | string | Button color variable (default: `"var:primary"`) |
| `widget.config` | object | Configuration element group |
| `widget.config.highlighted` | boolean | Whether this plan is featured/highlighted (default: `false`) |
| `widget.config.highlight_color` | string | Border/accent color when highlighted (default: `"var:primary"`) |
| `widget.config.highlight_color:hover` | string\|null | Highlight color on hover (default: `null`) |
| `widget.features` | array | Feature list items |
| `widget.features[].text` | object | Multilingual feature text |
| `widget.features[].included` | boolean | Whether feature is included |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "pricing-table",
  "uuid": "pricing-123",
  "widget": {
    "heading": {
      "title": { "en": "Pro", "pl": "Pro" },
      "subtitle": { "en": "For growing teams", "pl": "Dla rosnących zespołów" },
      "price": { "en": "$29", "pl": "119 zł" },
      "period": { "en": "month", "pl": "miesiąc" }
    },
    "badge": {
      "badge": { "en": "Most Popular", "pl": "Najpopularniejszy" }
    },
    "button": {
      "text": { "en": "Get Started", "pl": "Rozpocznij" },
      "style": "primary",
      "size": "md",
      "border_radius": "md",
      "padding": "",
      "icon": "",
      "icon_position": "left",
      "color": "var:primary"
    },
    "config": {
      "highlighted": true,
      "highlight_color": "var:primary",
      "highlight_color:hover": null
    },
    "features": [
      { "text": { "en": "10 Projects" }, "included": true },
      { "text": { "en": "Priority Support" }, "included": true },
      { "text": { "en": "Custom Domain" }, "included": true },
      { "text": { "en": "API Access" }, "included": false }
    ]
  },
  "settings": {}
}
```

## Usage Example

```javascript
function renderPricingTable(widget, language) {
  const { heading, badge, button, config, features } = widget.widget;
  const title = heading.title?.[language] || heading.title?.en || '';
  const subtitle = heading.subtitle?.[language] || heading.subtitle?.en || '';
  const price = heading.price?.[language] || heading.price?.en || '';
  const period = heading.period?.[language] || heading.period?.en || '';
  const buttonText = button.text?.[language] || button.text?.en || '';
  const badgeText = badge.badge?.[language] || badge.badge?.en || '';
  const buttonUrl = widget.widget.button_url || '#';

  const featuresHtml = (features || []).map(f => {
    const text = f.text?.[language] || f.text?.en || '';
    const icon = f.included !== false ? 'fa-check' : 'fa-times';
    const cls = f.included !== false ? '' : ' feature--excluded';
    return `<li class="feature${cls}"><i class="fas ${icon}"></i> ${text}</li>`;
  }).join('');

  const highlightColor = config.highlight_color || '';

  return `
    <div class="pricing${config.highlighted ? ' pricing--highlighted' : ''}" style="${config.highlighted ? `border-color: ${highlightColor}` : ''}">
      ${badgeText ? `<span class="pricing__badge" style="background: ${highlightColor}">${badgeText}</span>` : ''}
      <h3>${title}</h3>
      ${subtitle ? `<p class="pricing__subtitle">${subtitle}</p>` : ''}
      <div class="pricing__price">${price}<span>/ ${period}</span></div>
      <ul class="pricing__features">${featuresHtml}</ul>
      <a href="${buttonUrl}" class="pricing__button" style="font-size: ${button.size === 'lg' ? '18px' : '14px'}">${buttonText}</a>
    </div>
  `;
}
```
