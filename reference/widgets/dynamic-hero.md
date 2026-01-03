# Dynamic Hero Widget

A hero section that dynamically pulls content from a collection entry.

## Widget Type

```
dynamic-hero
```

## Data Properties

| Property | Type | Global | Description |
|----------|------|--------|-------------|
| `collection_code` | string | Yes | Collection code |
| `entry_source` | string | Yes | Entry source: `static` or `url` |
| `entry_id` | string | Yes | Specific entry ID (when static) |
| `entry_url_segment` | number | Yes | URL segment index (when url) |
| `image_field` | string | Yes | Field code for background image |
| `title_field` | string | Yes | Field code for title |
| `subtitle_field` | string | Yes | Field code for subtitle |
| `show_title` | boolean | Yes | Display title |
| `show_subtitle` | boolean | Yes | Display subtitle |
| `min_height` | number | Yes | Minimum height in pixels |
| `overlay_opacity` | number | Yes | Overlay opacity (0-1) |
| `overlay_color` | string | Yes | Overlay color (hex code) |
| `text_align` | string | Yes | Text alignment: `left`, `center`, `right` |
| `text_position` | string | Yes | Vertical position: `top`, `center`, `bottom` |
| `text_color` | string | Yes | Text color (hex code) |

## Example Response

```json
{
  "widget_type": "dynamic-hero",
  "uuid": "dhero-123",
  "data": {
    "collection_code": "blog",
    "entry_source": "url",
    "entry_url_segment": 2,
    "image_field": "featured_image",
    "title_field": "title",
    "subtitle_field": "excerpt",
    "show_title": true,
    "show_subtitle": true,
    "min_height": 500,
    "overlay_opacity": 0.5,
    "overlay_color": "#000000",
    "text_align": "center",
    "text_position": "center",
    "text_color": "#FFFFFF"
  },
  "settings": {}
}
```

## Entry Source Values

| Value | Description |
|-------|-------------|
| `static` | Use specific `entry_id` |
| `url` | Extract entry ID from URL segment |

## Text Position Values

| Value | Description |
|-------|-------------|
| `top` | Text at top of hero |
| `center` | Text centered vertically |
| `bottom` | Text at bottom of hero |

## Usage Example

```javascript
// Render dynamic hero widget
async function renderDynamicHero(widget, language, urlSegments, api) {
  const {
    collection_code, entry_source, entry_id, entry_url_segment,
    image_field, title_field, subtitle_field,
    show_title, show_subtitle,
    min_height, overlay_opacity, overlay_color,
    text_align, text_position, text_color
  } = widget.data;

  // Determine entry ID
  let targetEntryId = entry_id;
  if (entry_source === 'url' && entry_url_segment) {
    targetEntryId = urlSegments[entry_url_segment - 1];
  }

  if (!targetEntryId) {
    return '';
  }

  // Fetch entry
  const entry = await api.getEntry(collection_code, targetEntryId);
  if (!entry) {
    return '';
  }

  // Get field values
  const image = entry.data[image_field]?.url || '';
  const title = entry.data[title_field]?.[language] || entry.data[title_field]?.en || '';
  const subtitle = entry.data[subtitle_field]?.[language] || entry.data[subtitle_field]?.en || '';

  // Map text position to CSS
  const alignItems = {
    top: 'flex-start',
    center: 'center',
    bottom: 'flex-end'
  };

  return `
    <section class="dynamic-hero" style="
      position: relative;
      min-height: ${min_height || 400}px;
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
        opacity: ${overlay_opacity || 0.5};
      "></div>
      <div class="hero-content" style="
        position: relative;
        z-index: 1;
        text-align: ${text_align || 'center'};
        color: ${text_color || '#FFFFFF'};
        padding: 60px 20px;
        max-width: 900px;
      ">
        ${show_title && title ? `<h1 class="hero-title">${title}</h1>` : ''}
        ${show_subtitle && subtitle ? `<p class="hero-subtitle">${subtitle}</p>` : ''}
      </div>
    </section>
  `;
}
```

## CSS Example

```css
.dynamic-hero {
  width: 100%;
  overflow: hidden;
}

.hero-title {
  font-size: 48px;
  font-weight: 700;
  margin: 0 0 20px;
  text-shadow: 0 2px 4px rgba(0,0,0,0.3);
}

.hero-subtitle {
  font-size: 20px;
  margin: 0;
  opacity: 0.9;
  line-height: 1.6;
  text-shadow: 0 1px 2px rgba(0,0,0,0.2);
}

/* Responsive */
@media (max-width: 768px) {
  .dynamic-hero {
    min-height: 300px !important;
  }

  .hero-title {
    font-size: 32px;
  }

  .hero-subtitle {
    font-size: 16px;
  }

  .hero-content {
    padding: 40px 16px !important;
  }
}
```

## Use Cases

- **Blog post headers**: Display post title and featured image
- **Product pages**: Show product name over product image
- **Category headers**: Dynamic category banners
- **Event pages**: Event details with event image
