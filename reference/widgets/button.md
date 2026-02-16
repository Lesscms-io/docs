# Button Widget

A button widget with multilingual text and configurable link target.

## Widget Type

```
button
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"button"` |
| `uuid` | string | Unique widget identifier |
| `config` | object | Widget configuration |
| `config.link_type` | string | `"custom"`, `"page"`, or `"entry"` |
| `config.style` | string | Button style: `"primary"`, `"secondary"`, etc. |
| `config.size` | string | Button size: `"sm"`, `"md"`, `"lg"` |
| `config.target_blank` | boolean | Open in new tab |
| `content` | object | Widget content |
| `content.text` | object | Multilingual button text |
| `data` | object | Link data |
| `data.url` | string | Resolved URL for custom links |
| `data.page_uuid` | string | Page UUID (for page links) |
| `data.page_code` | string | Page code (for page links) |
| `data.entry_uuid` | string | Entry UUID (for entry links) |
| `data.collection_code` | string | Collection code (for entry links) |
| `data.entry_code` | string | Entry code (for entry links) |
| `settings` | object | Style settings (optional) |

## Example Response (Custom URL)

```json
{
  "widget_type": "button",
  "uuid": "btn-123",
  "config": {
    "link_type": "custom",
    "style": "primary",
    "size": "md",
    "target_blank": false
  },
  "content": {
    "text": {
      "en": "Learn More",
      "pl": "Dowiedz się więcej"
    }
  },
  "data": {
    "url": "https://example.com/contact"
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

## Example Response (Page Link)

```json
{
  "widget_type": "button",
  "uuid": "btn-456",
  "config": {
    "link_type": "page",
    "style": "secondary",
    "size": "lg",
    "target_blank": false
  },
  "content": {
    "text": {
      "en": "Contact Us",
      "pl": "Kontakt"
    }
  },
  "data": {
    "page_uuid": "abc-123",
    "page_code": "contact",
    "url": "/contact"
  },
  "settings": {}
}
```

## Example Response (Entry Link)

```json
{
  "widget_type": "button",
  "uuid": "btn-789",
  "config": {
    "link_type": "entry",
    "style": "primary",
    "size": "md",
    "target_blank": false
  },
  "content": {
    "text": {
      "en": "Read Article",
      "pl": "Czytaj artykuł"
    }
  },
  "data": {
    "entry_uuid": "entry-123",
    "collection_code": "blog",
    "entry_code": "my-article",
    "url": "/blog/my-article"
  },
  "settings": {}
}
```

## Link Types

| Value | Description |
|-------|-------------|
| `custom` | Custom URL (use `data.url`) |
| `page` | Link to a page (use `data.url` or `data.page_code`) |
| `entry` | Link to a collection entry (use `data.url` or `data.entry_code`) |

## Button Styles

| Value | Description |
|-------|-------------|
| `primary` | Primary action button |
| `secondary` | Secondary action button |
| `outline` | Outlined button |
| `link` | Text link style |

## Multi-Item Support

The button widget supports displaying multiple buttons in a grid. When multiple items are configured, the API returns a multi-item structure with `multi_item: true`, `multi_columns`, and `items[]` array. Each item has the same structure as a single button widget (without `uuid` and `settings`).

```json
{
  "widget_type": "button",
  "uuid": "btn-multi",
  "multi_item": true,
  "multi_columns": 2,
  "multi_gap": 16,
  "items": [
    {
      "widget_type": "button",
      "config": { "link_type": "custom", "style": "primary", "size": "md", "target_blank": false },
      "content": { "text": { "en": "Learn More" } },
      "data": { "url": "/about" }
    },
    {
      "widget_type": "button",
      "config": { "link_type": "custom", "style": "outline", "size": "md", "target_blank": false },
      "content": { "text": { "en": "Contact" } },
      "data": { "url": "/contact" }
    }
  ],
  "settings": {}
}
```

## Usage Example

```javascript
function renderButton(widget, language) {
  const { style, size, target_blank } = widget.config;
  const text = widget.content?.text?.[language] || widget.content?.text?.en || '';
  const url = widget.data?.url || '#';
  const target = target_blank ? ' target="_blank" rel="noopener"' : '';

  return `<a href="${url}" class="btn btn-${style} btn-${size}"${target}>${text}</a>`;
}

function renderButtonWidget(widget, language) {
  if (widget.multi_item) {
    return `<div style="display: grid; grid-template-columns: repeat(${widget.multi_columns}, 1fr); gap: ${widget.multi_gap}px;">
      ${widget.items.map(item => renderButton(item, language)).join('')}
    </div>`;
  }
  return renderButton(widget, language);
}
```
