# Pricing Table Widget

A pricing card with title, price, feature list and call-to-action button. Ideal for displaying subscription plans or service tiers.

## Widget Type

```
pricing-table
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"pricing-table"` |
| `uuid` | string | Unique widget identifier |
| `config` | object | Widget configuration |
| `config.highlighted` | boolean | Whether this plan is featured/highlighted |
| `config.highlight_color` | string | Border/accent color when highlighted |
| `config.button_color` | string | CTA button color |
| `config.button_url` | string | CTA button URL |
| `config.button_style` | string\|null | Button style name (Bootstrap variant) (default: `null`) |
| `config.button_size` | string | Button size: `"sm"`, `"md"`, `"lg"` (default: `"md"`) |
| `config.button_border_radius` | string | Button border radius preset: `"none"`, `"sm"`, `"md"`, `"lg"`, `"full"` (default: `"md"`) |
| `config.button_padding` | string\|null | Custom button padding CSS value (default: `null`) |
| `config.button_icon` | string\|null | Button icon class (Font Awesome) (default: `null`) |
| `config.button_icon_position` | string | Button icon position: `"left"`, `"right"` (default: `"left"`) |
| `config.button_link_type` | string | Button link type: `"custom"`, `"page"`, `"entry"` (default: `"custom"`) |
| `config.button_target_blank` | boolean | Open button link in new tab (default: `false`) |
| `config.button_page_id` | string\|null | Page UUID for page link type (default: `null`) |
| `config.button_entry_id` | string\|null | Entry UUID for entry link type (default: `null`) |
| `config.button_collection_code` | string\|null | Collection code for entry link type (default: `null`) |
| `config.button_route_uuid` | string\|null | Route UUID for link URL resolution (default: `null`) |
| `config.features` | array | Feature list items |
| `config.features[].text` | object | Multilingual feature text |
| `config.features[].included` | boolean | Whether feature is included |
| `content` | object | Multilingual content |
| `content.title` | object | Plan name |
| `content.subtitle` | object | Plan description |
| `content.price` | object | Price display (e.g., "$29") |
| `content.period` | object | Billing period (e.g., "month") |
| `content.button_text` | object | CTA button text |
| `content.badge` | object | Badge text (e.g., "Most Popular") |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "pricing-table",
  "uuid": "pricing-123",
  "config": {
    "highlighted": true,
    "highlight_color": "#50a5f1",
    "button_color": "#50a5f1",
    "button_url": "/signup/pro",
    "features": [
      { "text": { "en": "10 Projects" }, "included": true },
      { "text": { "en": "Priority Support" }, "included": true },
      { "text": { "en": "Custom Domain" }, "included": true },
      { "text": { "en": "API Access" }, "included": false }
    ]
  },
  "content": {
    "title": { "en": "Pro", "pl": "Pro" },
    "subtitle": { "en": "For growing teams", "pl": "Dla rosnących zespołów" },
    "price": { "en": "$29", "pl": "119 zł" },
    "period": { "en": "month", "pl": "miesiąc" },
    "button_text": { "en": "Get Started", "pl": "Rozpocznij" },
    "badge": { "en": "Most Popular", "pl": "Najpopularniejszy" }
  },
  "settings": {}
}
```

## Multi-Item Support

The pricing-table widget supports displaying multiple plans in a grid. When multiple items are configured, the API returns a multi-item structure:

```json
{
  "widget_type": "pricing-table",
  "uuid": "pricing-multi",
  "multi_item": true,
  "multi_columns": 3,
  "multi_gap": 16,
  "items": [
    {
      "widget_type": "pricing-table",
      "config": { "highlighted": false, "highlight_color": "#50a5f1", "button_color": "#50a5f1", "button_url": "/signup/basic", "features": [{ "text": { "en": "3 Projects" }, "included": true }] },
      "content": { "title": { "en": "Basic" }, "price": { "en": "$9" }, "period": { "en": "month" }, "button_text": { "en": "Start Free" }, "badge": {} }
    },
    {
      "widget_type": "pricing-table",
      "config": { "highlighted": true, "highlight_color": "#50a5f1", "button_color": "#50a5f1", "button_url": "/signup/pro", "features": [{ "text": { "en": "10 Projects" }, "included": true }] },
      "content": { "title": { "en": "Pro" }, "price": { "en": "$29" }, "period": { "en": "month" }, "button_text": { "en": "Get Started" }, "badge": { "en": "Most Popular" } }
    },
    {
      "widget_type": "pricing-table",
      "config": { "highlighted": false, "highlight_color": "#50a5f1", "button_color": "#50a5f1", "button_url": "/signup/enterprise", "features": [{ "text": { "en": "Unlimited" }, "included": true }] },
      "content": { "title": { "en": "Enterprise" }, "price": { "en": "$99" }, "period": { "en": "month" }, "button_text": { "en": "Contact Us" }, "badge": {} }
    }
  ],
  "settings": {}
}
```

Per-item fields (`title`, `subtitle`, `price`, `period`, `features`, `button_text`, `button_url`, `highlighted`, `badge`) are unique to each item. Shared fields (`highlight_color`, `button_color`) are the same across all items.

## Usage Example

```javascript
function renderPricingTable(widget, language) {
  const { highlighted, highlight_color, button_color, button_url, features } = widget.config;
  const title = widget.content?.title?.[language] || widget.content?.title?.en || '';
  const subtitle = widget.content?.subtitle?.[language] || widget.content?.subtitle?.en || '';
  const price = widget.content?.price?.[language] || widget.content?.price?.en || '';
  const period = widget.content?.period?.[language] || widget.content?.period?.en || '';
  const buttonText = widget.content?.button_text?.[language] || widget.content?.button_text?.en || '';
  const badge = widget.content?.badge?.[language] || widget.content?.badge?.en || '';

  const featuresHtml = (features || []).map(f => {
    const text = f.text?.[language] || f.text?.en || '';
    const icon = f.included !== false ? 'fa-check' : 'fa-times';
    const cls = f.included !== false ? '' : ' feature--excluded';
    return `<li class="feature${cls}"><i class="fas ${icon}"></i> ${text}</li>`;
  }).join('');

  return `
    <div class="pricing${highlighted ? ' pricing--highlighted' : ''}" style="${highlighted ? `border-color: ${highlight_color}` : ''}">
      ${badge ? `<span class="pricing__badge" style="background: ${highlight_color}">${badge}</span>` : ''}
      <h3>${title}</h3>
      ${subtitle ? `<p class="pricing__subtitle">${subtitle}</p>` : ''}
      <div class="pricing__price">${price}<span>/ ${period}</span></div>
      <ul class="pricing__features">${featuresHtml}</ul>
      <a href="${button_url}" class="pricing__button" style="background: ${button_color}">${buttonText}</a>
    </div>
  `;
}
```
