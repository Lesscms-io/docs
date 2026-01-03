# Widgets Reference

Widgets are the building blocks of page content in LessCMS. Each widget has specific data properties and common settings.

## Widget Categories

| Category | Description |
|----------|-------------|
| **Basic** | Button, Icon, Divider, Spacer, Icon Box, Key-Value, Star Rating |
| **Text** | Text, Heading, Blockquote, Icon List |
| **Media** | Image, Gallery, Video, Google Maps, Image Carousel |
| **Layout** | Hero, Toggle, Grid |
| **Interactive** | Countdown, Counter, Progress Bar, Testimonial, Alert |
| **Navigation** | Menu, Social Icons |
| **Collections** | Collection Grid, Collection Carousel, Collection Single, Value List, Collection Field, Dynamic Hero, Collection Grouped |

## Common Widget Structure

Every widget in the API response follows this structure:

```json
{
  "widget_type": "heading",
  "uuid": "widget-uuid-123",
  "data": {
    // Widget-specific data (see individual widget docs)
  },
  "settings": {
    // Common styling settings (see below)
  }
}
```

## Data Property Types

Widget data properties can be:

| Type | Description | Example |
|------|-------------|---------|
| **Global** | Same value for all languages | `"style": "primary"` |
| **Multilingual** | Object with language keys | `"text": {"en": "Hello", "pl": "Cześć"}` |

## Common Widget Settings

All widgets can include these settings for styling:

### Spacing

```json
{
  "settings": {
    "paddingTop": 20,
    "paddingRight": 20,
    "paddingBottom": 20,
    "paddingLeft": 20,
    "marginTop": 0,
    "marginRight": 0,
    "marginBottom": 0,
    "marginLeft": 0
  }
}
```

### Background

```json
{
  "settings": {
    "backgroundColor": "#ffffff",
    "backgroundOpacity": 100,
    "backgroundImage": "https://example.com/image.jpg",
    "backgroundSize": "cover",
    "backgroundPosition": "center center"
  }
}
```

### Border

```json
{
  "settings": {
    "borderRadius": 8,
    "borderWidth": 1,
    "borderColor": "#e0e0e0",
    "borderStyle": "solid",
    "boxShadow": "0 2px 4px rgba(0,0,0,0.1)"
  }
}
```

### Size

```json
{
  "settings": {
    "width": "100%",
    "maxWidth": 1200,
    "minWidth": 0,
    "height": "auto",
    "maxHeight": null,
    "minHeight": 0
  }
}
```

### Alignment

```json
{
  "settings": {
    "textAlign": "left",
    "verticalAlign": "center",
    "horizontalAlign": "center"
  }
}
```

### Custom CSS

```json
{
  "settings": {
    "cssClass": "my-custom-class",
    "customCss": ".my-custom-class { font-weight: bold; }"
  }
}
```

### Link Settings

Any widget can be made clickable by enabling link settings. This wraps the entire widget in an anchor tag.

```json
{
  "settings": {
    "link": {
      "enabled": true,
      "type": "custom",
      "url": "https://example.com",
      "target_blank": false
    }
  }
}
```

#### Link Types

| Type | Description | Required Fields |
|------|-------------|-----------------|
| `custom` | Custom URL | `url` |
| `page` | Internal page link | `page_id` (resolved to URL by API) |
| `entry` | Collection entry link | `collection_code`, `entry_id` (resolved to URL by API) |
| `route` | Named route link | `route_uuid` (resolved to URL by API) |

#### Link Properties

| Property | Type | Description |
|----------|------|-------------|
| `enabled` | boolean | Whether the link is active |
| `type` | string | Link type: `custom`, `page`, `entry`, `route` |
| `url` | string | Final resolved URL (always present when link is enabled) |
| `target_blank` | boolean | Open link in new tab |
| `page_id` | string | Page UUID (for `page` type) |
| `collection_code` | string | Collection code (for `entry` type) |
| `entry_id` | string | Entry UUID (for `entry` type) |
| `route_uuid` | string | Route UUID (for `route` type) |

#### Usage Example

```javascript
function renderWidget(widget, language) {
  const content = renderWidgetContent(widget, language);
  const link = widget.settings?.link;

  if (link?.enabled && link?.url) {
    const target = link.target_blank ? ' target="_blank" rel="noopener noreferrer"' : '';
    return `<a href="${link.url}"${target} class="lcms-widget-link">${content}</a>`;
  }

  return content;
}
```

#### CSS for Clickable Widgets

```css
.lcms-widget-link {
  display: block;
  text-decoration: none;
  color: inherit;
  cursor: pointer;
}

.lcms-widget-link:hover {
  opacity: 0.9;
}
```

### Responsive Overrides

Settings can have responsive overrides for tablet (768-1199px) and mobile (0-767px):

```json
{
  "settings": {
    "paddingTop": 40,
    "paddingBottom": 40,
    "responsive": {
      "tablet": {
        "paddingTop": 30,
        "paddingBottom": 30
      },
      "mobile": {
        "paddingTop": 20,
        "paddingBottom": 20
      }
    }
  }
}
```

## Widget List

### Basic Widgets
- [Button](widgets/button.md)
- [Icon](widgets/icon.md)
- [Divider](widgets/divider.md)
- [Spacer](widgets/spacer.md)
- [Icon Box](widgets/icon-box.md)
- [Key-Value](widgets/key-value.md)
- [Star Rating](widgets/star-rating.md)

### Text Widgets
- [Text](widgets/text.md)
- [Heading](widgets/heading.md)
- [Blockquote](widgets/blockquote.md)
- [Icon List](widgets/icon-list.md)

### Media Widgets
- [Image](widgets/image.md)
- [Gallery](widgets/gallery.md)
- [Video](widgets/video.md)
- [Google Maps](widgets/google-maps.md)
- [Image Carousel](widgets/image-carousel.md)

### Layout Widgets
- [Hero](widgets/hero.md)
- [Toggle](widgets/toggle.md)
- [Grid](widgets/grid.md)

### Interactive Widgets
- [Countdown](widgets/countdown.md)
- [Counter](widgets/counter.md)
- [Progress Bar](widgets/progress-bar.md)
- [Testimonial](widgets/testimonial.md)
- [Alert](widgets/alert.md)

### Navigation Widgets
- [Menu](widgets/menu.md)
- [Social Icons](widgets/social-icons.md)

### Collection Widgets
- [Collection Grid](widgets/collection-grid.md)
- [Collection Carousel](widgets/collection-carousel.md)
- [Collection Single](widgets/collection-single.md)
- [Value List](widgets/value-list.md)
- [Collection Field](widgets/collection-field.md)
- [Dynamic Hero](widgets/dynamic-hero.md)
- [Collection Grouped](widgets/collection-grouped.md)
