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
| `widget` | object | Widget data |
| `widget.highlighted` | boolean | Whether this plan is featured/highlighted |
| `widget.highlight_color` | string | Border/accent color when highlighted |
| `widget.button_color` | string | CTA button color |
| `widget.button_url` | string | CTA button URL |
| `widget.button_style` | string\|null | Button style name (Bootstrap variant) (default: `null`) |
| `widget.button_size` | string | Button size: `"sm"`, `"md"`, `"lg"` (default: `"md"`) |
| `widget.button_border_radius` | string | Button border radius preset: `"none"`, `"sm"`, `"md"`, `"lg"`, `"full"` (default: `"md"`) |
| `widget.button_padding` | string\|null | Custom button padding CSS value (default: `null`) |
| `widget.button_icon` | string\|null | Button icon class (Font Awesome) (default: `null`) |
| `widget.button_icon_position` | string | Button icon position: `"left"`, `"right"` (default: `"left"`) |
| `widget.button_link_type` | string | Button link type: `"custom"`, `"page"`, `"entry"` (default: `"custom"`) |
| `widget.button_target_blank` | boolean | Open button link in new tab (default: `false`) |
| `widget.button_page_id` | string\|null | Page UUID for page link type (default: `null`) |
| `widget.button_entry_id` | string\|null | Entry UUID for entry link type (default: `null`) |
| `widget.button_collection_code` | string\|null | Collection code for entry link type (default: `null`) |
| `widget.button_route_uuid` | string\|null | Route UUID for link URL resolution (default: `null`) |
| `widget.features` | array | Feature list items |
| `widget.features[].text` | object | Multilingual feature text |
| `widget.features[].included` | boolean | Whether feature is included |
| `widget.title` | object | Plan name |
| `widget.subtitle` | object | Plan description |
| `widget.price` | object | Price display (e.g., "$29") |
| `widget.period` | object | Billing period (e.g., "month") |
| `widget.button_text` | object | CTA button text |
| `widget.badge` | object | Badge text (e.g., "Most Popular") |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "pricing-table",
  "uuid": "pricing-123",
  "widget": {
    "highlighted": true,
    "highlight_color": "#50a5f1",
    "button_color": "#50a5f1",
    "button_url": "/signup/pro",
    "button_link_type": "page",
    "button_target_blank": false,
    "button_page_id": null,
    "button_entry_id": null,
    "button_collection_code": null,
    "button_route_uuid": null,
    "features": [
      { "text": { "en": "10 Projects" }, "included": true },
      { "text": { "en": "Priority Support" }, "included": true },
      { "text": { "en": "Custom Domain" }, "included": true },
      { "text": { "en": "API Access" }, "included": false }
    ],
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
      "widget": { "highlighted": false, "highlight_color": "#50a5f1", "button_color": "#50a5f1", "button_url": "/signup/basic", "features": [{ "text": { "en": "3 Projects" }, "included": true }], "title": { "en": "Basic" }, "price": { "en": "$9" }, "period": { "en": "month" }, "button_text": { "en": "Start Free" }, "badge": {} }
    },
    {
      "widget_type": "pricing-table",
      "widget": { "highlighted": true, "highlight_color": "#50a5f1", "button_color": "#50a5f1", "button_url": "/signup/pro", "features": [{ "text": { "en": "10 Projects" }, "included": true }], "title": { "en": "Pro" }, "price": { "en": "$29" }, "period": { "en": "month" }, "button_text": { "en": "Get Started" }, "badge": { "en": "Most Popular" } }
    },
    {
      "widget_type": "pricing-table",
      "widget": { "highlighted": false, "highlight_color": "#50a5f1", "button_color": "#50a5f1", "button_url": "/signup/enterprise", "features": [{ "text": { "en": "Unlimited" }, "included": true }], "title": { "en": "Enterprise" }, "price": { "en": "$99" }, "period": { "en": "month" }, "button_text": { "en": "Contact Us" }, "badge": {} }
    }
  ],
  "settings": {}
}
```

Per-item fields (`title`, `subtitle`, `price`, `period`, `features`, `button_text`, `button_url`, `highlighted`, `badge`) are unique to each item. Shared fields (`highlight_color`, `button_color`) are the same across all items.

## Usage Example

```javascript
function renderPricingTable(widget, language) {
  const { highlighted, highlight_color, button_color, button_url, features } = widget.widget;
  const title = widget.widget?.title?.[language] || widget.widget?.title?.en || '';
  const subtitle = widget.widget?.subtitle?.[language] || widget.widget?.subtitle?.en || '';
  const price = widget.widget?.price?.[language] || widget.widget?.price?.en || '';
  const period = widget.widget?.period?.[language] || widget.widget?.period?.en || '';
  const buttonText = widget.widget?.button_text?.[language] || widget.widget?.button_text?.en || '';
  const badge = widget.widget?.badge?.[language] || widget.widget?.badge?.en || '';

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
