# Hero Widget

A full-width hero section with background image, title, subtitle, and optional call-to-action button. Supports both static content and dynamic content from collection entries.

## Widget Type

```
hero
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"hero"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget data |
| `widget.content_source` | string | Content source: `"static"` or `"dynamic"` (default: `"static"`) |
| `widget.overlay_opacity` | number | Background overlay opacity 0-1 (default: 0.4) |
| `widget.overlay_color` | string | Background overlay color (default: `"#000000"`) |
| `widget.text_align` | string | Text alignment: `"left"`, `"center"`, `"right"` (default: `"center"`) |
| `widget.text_position` | string | Vertical text position: `"top"`, `"center"`, `"bottom"` (default: `"center"`) |
| `widget.text_color` | string | Text color (default: `"#ffffff"`) |
| `widget.button_style` | string | Button style variant (default: `"primary"`) |
| `widget.button_size` | string | Button size: `"sm"`, `"md"`, `"lg"` (default: `"lg"`) |
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
| `widget.hover_overlay_color` | string\|null | Overlay color on hover |
| `widget.hover_text_color` | string\|null | Text color on hover |
| `widget.hover_lift` | number | Hover lift in pixels — translateY offset (default: `0`) |
| `widget.hover_scale` | number | Hover scale factor (default: `1`) |
| `widget.hover_shadow` | string | Hover shadow preset: `"none"`, `"sm"`, `"md"`, `"lg"` (default: `"none"`) |
| `widget.transition_duration` | number | Hover transition duration in ms (default: 200) |
| `settings` | object | Style settings (optional) |

### Static Mode Content

When `content_source` is `"static"`:

| Property | Type | Description |
|----------|------|-------------|
| `widget.title` | object | Multilingual title text `{ "en": "...", "pl": "..." }` |
| `widget.subtitle` | object | Multilingual subtitle text |
| `widget.background_url` | string\|null | Background image URL |
| `widget.button_text` | object | Multilingual CTA button text |
| `widget.button_url` | string | CTA button URL (default: `"#"`) |

### Dynamic Mode Fields

When `content_source` is `"dynamic"`:

| Property | Type | Description |
|----------|------|-------------|
| `widget.collection_code` | string\|null | Collection code |
| `widget.entry_source` | string | Entry source: `"static"` or `"url"` (default: `"static"`) |
| `widget.entry_id` | string\|null | Specific entry ID |
| `widget.entry_url_segment` | number | URL segment index (default: 1) |
| `widget.image_field` | string\|null | Field code for background image |
| `widget.title_field` | string\|null | Field code for title |
| `widget.subtitle_field` | string\|null | Field code for subtitle |
| `widget.show_title` | boolean | Show title (default: true) |
| `widget.show_subtitle` | boolean | Show subtitle (default: false) |
| `widget.button_text` | object | Multilingual CTA button text |
| `widget.button_url` | string | CTA button URL |

## Example Response (Static Mode)

```json
{
  "widget_type": "hero",
  "uuid": "hero-123",
  "widget": {
    "content_source": "static",
    "overlay_opacity": 0.4,
    "overlay_color": "#000000",
    "text_align": "center",
    "text_position": "center",
    "text_color": "#ffffff",
    "button_style": "primary",
    "button_size": "lg",
    "button_border_radius": "md",
    "button_padding": null,
    "button_icon": null,
    "button_icon_position": "left",
    "button_link_type": "custom",
    "button_target_blank": false,
    "button_page_id": null,
    "button_entry_id": null,
    "button_collection_code": null,
    "button_route_uuid": null,
    "hover_overlay_color": null,
    "hover_text_color": null,
    "hover_lift": 0,
    "hover_scale": 1,
    "hover_shadow": "none",
    "transition_duration": 200,
    "title": {
      "en": "Welcome to Our Website",
      "pl": "Witamy na naszej stronie"
    },
    "subtitle": {
      "en": "We build amazing digital experiences.",
      "pl": "Tworzymy niesamowite cyfrowe doswiadczenia."
    },
    "background_url": "https://cdn.example.com/hero-bg.jpg",
    "button_text": {
      "en": "Get Started",
      "pl": "Rozpocznij"
    },
    "button_url": "/contact"
  },
  "settings": {
    "minHeight": 600,
    "horizontalAlign": "center",
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Example Response (Dynamic Mode)

```json
{
  "widget_type": "hero",
  "uuid": "hero-456",
  "widget": {
    "content_source": "dynamic",
    "collection_code": "products",
    "entry_source": "url",
    "entry_id": null,
    "entry_url_segment": 2,
    "image_field": "featured_image",
    "title_field": "name",
    "subtitle_field": "short_description",
    "show_title": true,
    "show_subtitle": true,
    "overlay_opacity": 0.5,
    "overlay_color": "#1a1a2e",
    "text_align": "left",
    "text_position": "bottom",
    "text_color": "#ffffff",
    "button_style": "primary",
    "button_size": "lg",
    "button_text": {
      "en": "Learn More",
      "pl": "Dowiedz sie wiecej"
    },
    "button_url": "#details"
  },
  "settings": {}
}
```

## Content Source Values

| Value | Description |
|-------|-------------|
| `static` | Use static content from `widget` object |
| `dynamic` | Fetch content from collection entry |

## Entry Source Values (Dynamic Mode)

| Value | Description |
|-------|-------------|
| `static` | Use specific `entry_id` |
| `url` | Extract entry ID from URL segment at `entry_url_segment` index |

## Text Align Values

| Value | Description |
|-------|-------------|
| `left` | Left-aligned text |
| `center` | Center-aligned text |
| `right` | Right-aligned text |

## Text Position Values

| Value | Description |
|-------|-------------|
| `top` | Content positioned at top |
| `center` | Content centered vertically |
| `bottom` | Content positioned at bottom |

## Usage Example (Static)

```javascript
function renderHero(widget, language) {
  const { content_source, overlay_opacity, overlay_color, text_align, text_position, text_color } = widget.widget;
  const { title, subtitle, background_url, button_text, button_url } = widget.widget;

  const titleText = title?.[language] || title?.en || '';
  const subtitleText = subtitle?.[language] || subtitle?.en || '';
  const buttonText = button_text?.[language] || button_text?.en || '';

  const alignItems = text_position === 'top' ? 'flex-start' : text_position === 'bottom' ? 'flex-end' : 'center';

  return `
    <section class="hero" style="
      position: relative;
      min-height: 500px;
      display: flex;
      align-items: ${alignItems};
      justify-content: center;
    ">
      <div class="hero-bg" style="
        position: absolute;
        inset: 0;
        background-image: url('${background_url || ''}');
        background-size: cover;
        background-position: center;
      "></div>
      <div class="hero-overlay" style="
        position: absolute;
        inset: 0;
        background-color: ${overlay_color};
        opacity: ${overlay_opacity};
      "></div>
      <div class="hero-content" style="
        position: relative;
        z-index: 1;
        text-align: ${text_align};
        color: ${text_color};
        padding: 2rem;
      ">
        ${titleText ? `<h1>${titleText}</h1>` : ''}
        ${subtitleText ? `<p class="hero-subtitle">${subtitleText}</p>` : ''}
        ${buttonText && button_url ? `
          <a href="${button_url}" class="btn btn-primary btn-lg">${buttonText}</a>
        ` : ''}
      </div>
    </section>
  `;
}
```

## Usage Example (Dynamic)

```javascript
async function renderDynamicHero(widget, language, urlSegments, api) {
  const {
    collection_code, entry_source, entry_id, entry_url_segment,
    image_field, title_field, subtitle_field,
    show_title, show_subtitle,
    overlay_opacity, overlay_color, text_align, text_position, text_color,
    button_text, button_url
  } = widget.widget;

  // Determine entry ID
  let targetEntryId = entry_id;
  if (entry_source === 'url' && entry_url_segment) {
    targetEntryId = urlSegments[entry_url_segment - 1];
  }

  if (!targetEntryId || !collection_code) {
    return '<section class="hero hero-empty">No content available</section>';
  }

  // Fetch entry from collection
  const entry = await api.getEntry(collection_code, targetEntryId);
  if (!entry) return '<section class="hero hero-empty">Entry not found</section>';

  // Extract field values
  const getFieldValue = (fieldCode) => {
    if (!fieldCode) return null;
    const value = entry.data[fieldCode];
    if (typeof value === 'object' && !Array.isArray(value)) {
      return value[language] || value.en || '';
    }
    return value;
  };

  const backgroundUrl = image_field ? getFieldValue(image_field) : null;
  const titleText = show_title && title_field ? getFieldValue(title_field) : '';
  const subtitleText = show_subtitle && subtitle_field ? getFieldValue(subtitle_field) : '';
  const buttonLabel = button_text?.[language] || button_text?.en || '';

  const alignItems = text_position === 'top' ? 'flex-start' : text_position === 'bottom' ? 'flex-end' : 'center';

  return `
    <section class="hero hero-dynamic" style="
      position: relative;
      min-height: 500px;
      display: flex;
      align-items: ${alignItems};
      justify-content: center;
    ">
      <div class="hero-bg" style="
        position: absolute;
        inset: 0;
        background-image: url('${backgroundUrl || ''}');
        background-size: cover;
        background-position: center;
      "></div>
      <div class="hero-overlay" style="
        position: absolute;
        inset: 0;
        background-color: ${overlay_color};
        opacity: ${overlay_opacity};
      "></div>
      <div class="hero-content" style="
        position: relative;
        z-index: 1;
        text-align: ${text_align};
        color: ${text_color};
        padding: 2rem;
      ">
        ${titleText ? `<h1>${titleText}</h1>` : ''}
        ${subtitleText ? `<p class="hero-subtitle">${subtitleText}</p>` : ''}
        ${buttonLabel && button_url ? `
          <a href="${button_url}" class="btn btn-primary btn-lg">${buttonLabel}</a>
        ` : ''}
      </div>
    </section>
  `;
}
```

## Use Cases

- **Landing page headers**: Full-width hero with static content
- **Product detail pages**: Dynamic hero pulling from collection entry
- **Category pages**: Hero with URL-based entry selection
- **Marketing pages**: Customizable overlays and text positioning
