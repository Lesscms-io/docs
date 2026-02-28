# Widgets Reference

Widgets are the building blocks of page content in LessCMS. Each widget has specific data properties and common settings.

## Widget Categories

| Category | Description |
|----------|-------------|
| **Basic** | Button, Divider, Spacer, Icon Box, Service Card, Link, Pill, Team Member, Pricing Table |
| **Text** | Text, Heading, Blockquote |
| **Media** | Image, Gallery, Video, PDF Viewer |
| **Layout** | Grid |
| **Interactive** | Hero, Countdown, Counter, Progress Bar, Testimonial, Alert, Accordion, Tabs, Timeline |
| **Cards** | CTA Box, Feature List, Icon List |
| **Navigation** | Menu |
| **Integrations** | Google Maps, Social Icons, Embed, Table |
| **Collections** | Collection Grid, Collection Carousel, Collection Single, Collection Grouped, Value List, Data Field |

## Common Widget Structure

Every widget in the API response follows this structure:

```json
{
  "widget_type": "heading",
  "uuid": "widget-uuid-123",
  "config": {
    // Widget-specific configuration
  },
  "content": {
    // Widget-specific content (localized)
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
| **Config** | Global settings, same for all languages | `"style": "primary"` |
| **Content** | Localized text content | `"text": "Hello"` (already resolved for requested language) |

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
- [Button](reference/widgets/button.md)
- [Divider](reference/widgets/divider.md)
- [Spacer](reference/widgets/spacer.md)
- [Icon Box](reference/widgets/icon-box.md)
- [Numbered Box](reference/widgets/numbered-box.md)
- [Service Card](reference/widgets/service-card.md)
- [Link](reference/widgets/link.md)
- [Pill](reference/widgets/pill.md)
- [Team Member](reference/widgets/team-member.md)
- [Pricing Table](reference/widgets/pricing-table.md)

### Text Widgets
- [Text](reference/widgets/text.md)
- [Heading](reference/widgets/heading.md)
- [Blockquote](reference/widgets/blockquote.md)

### Media Widgets
- [Image](reference/widgets/image.md)
- [Gallery](reference/widgets/gallery.md)
- [Video](reference/widgets/video.md)
- [PDF Viewer](reference/widgets/pdf-viewer.md)

### Layout Widgets
- [Grid](reference/widgets/grid.md)

### Interactive Widgets
- [Hero](reference/widgets/hero.md)
- [Countdown](reference/widgets/countdown.md)
- [Counter](reference/widgets/counter.md)
- [Progress Bar](reference/widgets/progress-bar.md)
- [Testimonial](reference/widgets/testimonial.md)
- [Alert](reference/widgets/alert.md)
- [Accordion](reference/widgets/accordion.md)
- [Tabs](reference/widgets/tabs.md)
- [Timeline](reference/widgets/timeline.md)

### Cards Widgets
- [CTA Box](reference/widgets/cta-box.md)
- [Feature List](reference/widgets/feature-list.md)
- [Icon List](reference/widgets/icon-list.md)

### Navigation Widgets
- [Menu](reference/widgets/menu.md)

### Integration Widgets
- [Google Maps](reference/widgets/google-maps.md)
- [Social Icons](reference/widgets/social-icons.md)
- [Embed](reference/widgets/embed.md)
- [Table](reference/widgets/table.md)

### Collection Widgets
- [Collection Grid](reference/widgets/collection-grid.md)
- [Collection Carousel](reference/widgets/collection-carousel.md)
- [Collection Single](reference/widgets/collection-single.md)
- [Collection Grouped](reference/widgets/collection-grouped.md)
- [Value List](reference/widgets/value-list.md)
- [Data Field](reference/widgets/data-field.md)
