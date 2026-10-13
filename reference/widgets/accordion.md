# Accordion Widget

An expandable/collapsible content widget with multiple items. Uses nested element-group structure.

## Widget Type

```
accordion
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"accordion"` |
| `uuid` | string | Unique widget identifier |
| `widget.header` | object | Header (title) styling |
| `widget.header.color` | string\|null | Title text color |
| `widget.header.color:hover` | string\|null | Title text color on hover |
| `widget.header.background` | string\|null | Header background color |
| `widget.header.background:hover` | string\|null | Header background on hover |
| `widget.header.tag` | string | HTML tag for title (h2-h6, p, span) |
| `widget.meta` | object | Meta text (columns layout: 2nd and 3rd column) |
| `widget.meta.color` | string\|null | Meta text color (also applies on hover, see `hover_color` in appearance) |
| `widget.index` | object | Item number (columns layout, shown when `config.show_index`) |
| `widget.index.style` | string | `outline` (contour, fills on hover/open) or `fill` |
| `widget.index.color` | string\|null | Number color (outline/fill) |
| `widget.index.color:hover` | string\|null | Number color on hover / when open |
| `widget.content` | object | Content area styling |
| `widget.content.color` | string\|null | Content text color |
| `widget.content.color:hover` | string\|null | Content text color on hover |
| `widget.content.border_color` | string\|null | Content area top border color |
| `widget.content.border_color:hover` | string\|null | Content border color on hover |
| `widget.content.tag` | string | HTML tag for content wrapper (p, div, span) |
| `widget.icon` | object | Expand/collapse icon |
| `widget.icon.color` | string\|null | Icon color |
| `widget.icon.color:hover` | string\|null | Icon color on hover |
| `widget.icon.open` | string | Icon class when item is open (e.g. `fa-solid fa-minus`) |
| `widget.icon.closed` | string | Icon class when item is closed (e.g. `fa-solid fa-plus`) |
| `widget.icon.background` | string\|null | Icon background (used by `plus-circle` style) |
| `widget.icon.background:hover` | string\|null | Icon background on hover / when open |
| `widget.border` | object | Item border |
| `widget.border.color` | string\|null | Border color around each item |
| `widget.border.color:hover` | string\|null | Item border color on hover |
| `widget.separator` | object | Separator line between items |
| `widget.separator.color` | string\|null | Separator line color |
| `widget.separator.color:hover` | string\|null | Separator color on hover |
| `widget.divider` | object | Row divider lines (columns layout) |
| `widget.divider.color` | string\|null | Divider line color |
| `widget.accent_line` | object | Accent line drawn across the top of a row on hover / open |
| `widget.accent_line.enabled` | boolean | Show the accent line |
| `widget.accent_line.color` | string\|null | Accent line color (default: primary) |
| `widget.config` | object | Behavior configuration |
| `widget.config.allow_multiple` | boolean | Allow multiple items open simultaneously |
| `widget.config.first_open` | boolean | First item open by default |
| `widget.config.header_layout` | string | `simple` (boxed rows) or `columns` (index / title / meta1 / meta2 / icon row, "zestawienie realizacji") |
| `widget.config.show_index` | boolean | Show item number (columns layout) |
| `widget.config.icon_style` | string | `chevron` (default, uses `icon.open` / `icon.closed`) or `plus-circle` (outlined round button, rotates to x when open) |
| `widget.config.content_type` | string | `text` (default), `gallery` or `text_gallery` |
| `widget.config.lightbox` | boolean | Gallery images open the lightbox on click (default: `true`) |
| `widget.config.lightbox_zoom` | boolean | Zoom inside the lightbox (default: `true`) |
| `widget.config.source` | string | `manual` (items below) or `collection` (entries of a collection, mapped through `config.map`) |
| `widget.config.collection` | string\|null | Collection code (when `source` = `collection`) |
| `widget.config.map` | object | Collection field mapping: `title`, `meta1`, `meta2`, `images`, `content` (field codes) |
| `widget.items` | array | List of accordion items |
| `widget.items[].title_html` | object | Multilingual item title |
| `widget.items[].html` | object | Multilingual item content — rich text HTML (paragraphs, bold, links, lists, images with class `lcms-rich-img`). Legacy items may still contain plain text; render with newline→paragraph fallback |
| `widget.items[].meta1_html` | object | Multilingual meta text 1 (e.g. city) |
| `widget.items[].meta2_html` | object | Multilingual meta text 2 (e.g. scope) |
| `widget.items[].images` | array | Gallery images `[{ "url": "...", "alt"?: "...", "type"?: "video" }]`. Rendered on click-to-open lightbox (zoom) |
| `settings` | object | [Shared widget settings](shared-settings.md) |

## Example Response

```json
{
  "widget_type": "accordion",
  "uuid": "acc-001",
  "widget": {
    "header": {
      "color": "var:dark",
      "color:hover": null,
      "background": null,
      "background:hover": null,
      "tag": "h3"
    },
    "content": {
      "color": "var:muted",
      "color:hover": null,
      "border_color": null,
      "border_color:hover": null,
      "tag": "p"
    },
    "icon": {
      "color": "var:primary",
      "color:hover": null,
      "open": "fa-solid fa-minus",
      "closed": "fa-solid fa-plus"
    },
    "border": {
      "color": null,
      "color:hover": null
    },
    "separator": {
      "color": "var:border",
      "color:hover": null
    },
    "meta": {
      "color": "var:muted"
    },
    "index": {
      "style": "outline",
      "color": "var:primary",
      "color:hover": "var:primary"
    },
    "divider": {
      "color": "var:border"
    },
    "accent_line": {
      "color": "var:primary",
      "enabled": false
    },
    "config": {
      "allow_multiple": false,
      "first_open": true,
      "header_layout": "simple",
      "show_index": false,
      "icon_style": "chevron",
      "content_type": "text",
      "source": "manual",
      "collection": null,
      "map": { "title": null, "meta1": null, "meta2": null, "images": null, "content": null }
    },
    "items": [
      {
        "title_html": {
          "en": "What is LessCMS?",
          "pl": "Czym jest LessCMS?"
        },
        "html": {
          "en": "LessCMS is a headless content management system.",
          "pl": "LessCMS to bezgłowy system zarządzania treścią."
        },
        "meta1_html": {},
        "meta2_html": {},
        "images": []
      },
      {
        "title_html": {
          "en": "How to get started?",
          "pl": "Jak zacząć?"
        },
        "html": {
          "en": "Sign up for a free account and create your first project.",
          "pl": "Zarejestruj się za darmo i stwórz swój pierwszy projekt."
        }
      }
    ]
  },
  "settings": {
    "padding_top": 12,
    "padding_bottom": 12,
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Collection source

With `config.source = "collection"` the API fetches entries of `config.collection` (up to 20, newest first) and builds `items[]` from them: `title_html` ← `config.map.title`, `meta1_html` ← `config.map.meta1`, `meta2_html` ← `config.map.meta2`, `html` ← `config.map.content`, `images` ← `config.map.images`. Gallery fields with a single image value are accepted too. Items stored under `widget.data.items` are ignored in this mode.

## Wrappable

No — accordion manages its own items list internally.

## Usage Example

```javascript
function renderAccordion(widget, language) {
  const { header, content, icon, border, separator, config, items } = widget.widget;
  const headerTag = header.tag || 'h3';
  const contentTag = content.tag || 'p';

  return `
    <div class="accordion" data-allow-multiple="${config.allow_multiple}">
      ${items.map((item, index) => {
        const title = item.title_html?.[language] || item.title_html?.en || '';
        const body = item.html?.[language] || item.html?.en || '';
        const isOpen = config.first_open && index === 0;
        const borderStyle = border.color ? `border: 1px solid ${border.color}; border-radius: 8px;` : '';
        const separatorHtml = index > 0 && separator.color
          ? `<div style="height: 1px; background: ${separator.color}"></div>` : '';

        return `
          ${separatorHtml}
          <div class="accordion-item" style="${borderStyle}">
            <button class="accordion-header" aria-expanded="${isOpen}"
              style="color: ${header.color || 'inherit'}; background: ${header.background || 'transparent'}">
              <${headerTag}>${title}</${headerTag}>
              <i class="${isOpen ? icon.open : icon.closed}"
                style="color: ${icon.color || 'inherit'}"></i>
            </button>
            <div class="accordion-body" ${isOpen ? '' : 'hidden'}
              style="color: ${content.color || 'inherit'}; border-top: 1px solid ${content.border_color || 'transparent'}">
              <${contentTag}>${body}</${contentTag}>
            </div>
          </div>
        `;
      }).join('')}
    </div>
  `;
}
```
