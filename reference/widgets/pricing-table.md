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
| `widget.heading.title_html` | object | Multilingual plan name (FE source: `heading.title`) |
| `widget.heading.subtitle_html` | object | Multilingual plan description (FE source: `heading.subtitle`) |
| `widget.heading.price_html` | object | Multilingual price display e.g. "$29" (FE source: `heading.price`) |
| `widget.heading.period_html` | object | Multilingual billing period e.g. "month" (FE source: `heading.period`) |
| `widget.badge` | object | Badge element group |
| `widget.badge.html` | object | Multilingual badge text e.g. "Most Popular" (FE source: `badge.badge`) |
| `widget.button` | object | Button element group |
| `widget.button.html` | object | Multilingual CTA button text |
| `widget.button.style` | string | Button style name (Bootstrap variant) (default: `"primary"`) |
| `widget.button.size` | string | Button size: `"sm"`, `"md"`, `"lg"` (default: `"md"`) |
| `widget.button.border_radius` | string | Button border radius preset: `"none"`, `"sm"`, `"md"`, `"lg"`, `"full"` (default: `"md"`) |
| `widget.button.padding` | string | Custom button padding CSS value (default: `""`) |
| `widget.button.icon` | string | Button icon class (Font Awesome) (default: `""`) |
| `widget.button.icon_position` | string | Button icon position: `"left"`, `"right"` (default: `"left"`) |
| `widget.button.color` | string | Button color variable (default: `"var:primary"`) |
| `widget.button_url` | string\|null | Resolved button URL (server-side resolved from page/entry/custom) |
| `widget.button_link_type` | string | Button link type: `"custom"`, `"page"`, `"entry"` (default: `"custom"`) |
| `widget.button_target_blank` | boolean | Open button link in new tab (default: `false`) |
| `widget.button_page_id` | string\|null | Button page UUID (only when `button_link_type` is `"page"`) |
| `widget.button_route_uuid` | string\|null | Button route UUID (only when `button_link_type` is `"page"` or `"entry"`) |
| `widget.button_entry_id` | string\|null | Button entry UUID (only when `button_link_type` is `"entry"`) |
| `widget.button_collection_code` | string\|null | Button collection code (only when `button_link_type` is `"entry"`) |
| `widget.config` | object | Configuration element group |
| `widget.config.highlighted` | boolean | Whether this plan is featured/highlighted (default: `false`) |
| `widget.config.highlight_color` | string | Border/accent color when highlighted (default: `"var:primary"`) |
| `widget.config.highlight_color:hover` | string\|null | Highlight color on hover (default: `null`) |
| `widget.features` | array | Feature list items |
| `widget.features[].html` | object | Multilingual feature text |
| `widget.features[].included` | boolean | Whether feature is included |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "pricing-table",
  "uuid": "pricing-123",
  "widget": {
    "heading": {
      "title_html": { "en": "Pro", "pl": "Pro" },
      "subtitle_html": { "en": "For growing teams", "pl": "Dla rosnących zespołów" },
      "price_html": { "en": "$29", "pl": "119 zł" },
      "period_html": { "en": "month", "pl": "miesiąc" }
    },
    "badge": {
      "html": { "en": "Most Popular", "pl": "Najpopularniejszy" }
    },
    "button": {
      "html": { "en": "Get Started", "pl": "Rozpocznij" },
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
      { "html": { "en": "10 Projects" }, "included": true },
      { "html": { "en": "Priority Support" }, "included": true },
      { "html": { "en": "Custom Domain" }, "included": true },
      { "html": { "en": "API Access" }, "included": false }
    ],
    "button_url": "/pricing",
    "button_link_type": "page",
    "button_target_blank": false,
    "button_page_id": "page-uuid-123",
    "button_route_uuid": "route-uuid-456",
    "button_entry_id": null,
    "button_collection_code": null
  },
  "settings": {}
}
```

## Usage Example

```javascript
function renderPricingTable(widget, language) {
  const { heading, badge, button, config, features } = widget.widget;
  const title = heading.title_html?.[language] || heading.title_html?.en || '';
  const subtitle = heading.subtitle_html?.[language] || heading.subtitle_html?.en || '';
  const price = heading.price_html?.[language] || heading.price_html?.en || '';
  const period = heading.period_html?.[language] || heading.period_html?.en || '';
  const buttonText = button.html?.[language] || button.html?.en || '';
  const badgeText = badge.html?.[language] || badge.html?.en || '';
  const buttonUrl = widget.widget.button_url || '#';

  const featuresHtml = (features || []).map(f => {
    const text = f.html?.[language] || f.html?.en || '';
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
