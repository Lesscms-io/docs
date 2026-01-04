# Dynamic Hero Widget

A hero section that dynamically pulls content from a collection entry.

## Widget Type

```
dynamic-hero
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"dynamic-hero"` |
| `uuid` | string | Unique widget identifier |
| `config` | object | Widget configuration |
| `config.collection_code` | string | Collection code |
| `config.entry_source` | string | `"static"` or `"url"` |
| `config.entry_id` | string | Entry ID (when static) |
| `config.entry_url_segment` | number | URL segment index (when url) |
| `config.image_field` | string | Field code for background image |
| `config.title_field` | string | Field code for title |
| `config.subtitle_field` | string | Field code for subtitle |
| `config.show_title` | boolean | Display title |
| `config.show_subtitle` | boolean | Display subtitle |
| `config.overlay_opacity` | number | Overlay opacity (0-1) |
| `config.overlay_color` | string | Overlay color (hex) |
| `config.text_align` | string | `"left"`, `"center"`, `"right"` |
| `config.text_position` | string | `"top"`, `"center"`, `"bottom"` |
| `config.text_color` | string | Text color (hex) |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "dynamic-hero",
  "uuid": "dhero-123",
  "config": {
    "collection_code": "blog",
    "entry_source": "url",
    "entry_id": null,
    "entry_url_segment": 2,
    "image_field": "featured_image",
    "title_field": "title",
    "subtitle_field": "excerpt",
    "show_title": true,
    "show_subtitle": true,
    "overlay_opacity": 0.4,
    "overlay_color": "#000000",
    "text_align": "center",
    "text_position": "center",
    "text_color": "#ffffff"
  },
  "settings": {
    "minHeight": 500,
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Entry Source Values

| Value | Description |
|-------|-------------|
| `static` | Use specific `entry_id` |
| `url` | Extract entry from URL segment |

## Text Position Values

| Value | Description |
|-------|-------------|
| `top` | Text at top of hero |
| `center` | Text centered vertically |
| `bottom` | Text at bottom of hero |

## Usage Example

```javascript
async function renderDynamicHero(widget, language, urlSegments, api) {
  const {
    collection_code, entry_source, entry_id, entry_url_segment,
    image_field, title_field, subtitle_field,
    show_title, show_subtitle,
    overlay_opacity, overlay_color,
    text_align, text_position, text_color
  } = widget.config;

  const minHeight = widget.settings?.minHeight || 400;

  // Determine entry ID
  let targetEntryId = entry_id;
  if (entry_source === 'url' && entry_url_segment) {
    targetEntryId = urlSegments[entry_url_segment - 1];
  }

  if (!targetEntryId) return '';

  // Fetch entry
  const entry = await api.getEntry(collection_code, targetEntryId);
  if (!entry) return '';

  // Get field values
  const image = entry.data[image_field]?.url || '';
  const title = entry.data[title_field]?.[language] || '';
  const subtitle = entry.data[subtitle_field]?.[language] || '';

  const alignItems = { top: 'flex-start', center: 'center', bottom: 'flex-end' };

  return `
    <section class="dynamic-hero" style="
      position: relative;
      min-height: ${minHeight}px;
      background-image: url('${image}');
      background-size: cover;
      background-position: center;
      display: flex;
      align-items: ${alignItems[text_position] || 'center'};
      justify-content: center;
    ">
      <div class="hero-overlay" style="
        position: absolute;
        inset: 0;
        background: ${overlay_color || '#000000'};
        opacity: ${overlay_opacity || 0.4};
      "></div>
      <div class="hero-content" style="
        position: relative;
        z-index: 1;
        text-align: ${text_align || 'center'};
        color: ${text_color || '#FFFFFF'};
        padding: 60px 20px;
        max-width: 900px;
      ">
        ${show_title && title ? `<h1>${title}</h1>` : ''}
        ${show_subtitle && subtitle ? `<p>${subtitle}</p>` : ''}
      </div>
    </section>
  `;
}
```

## Use Cases

- **Blog post headers**: Display post title and featured image
- **Product pages**: Show product name over product image
- **Category headers**: Dynamic category banners
- **Event pages**: Event details with event image
