# Collection Field Widget

Display a single field from a collection entry with customizable rendering.

## Widget Type

```
collection-field
```

## Data Properties

| Property | Type | Global | Description |
|----------|------|--------|-------------|
| `collection_code` | string | Yes | Collection code |
| `entry_source` | string | Yes | Entry source: `static` or `url` |
| `entry_id` | string | Yes | Specific entry ID (when static) |
| `entry_url_segment` | number | Yes | URL segment index (when url) |
| `field_code` | string | Yes | Field code to display |
| `display_as` | string | Yes | Render as: `p`, `h1`-`h6`, `span`, `image`, `gallery` |

## Example Response (Static Entry)

```json
{
  "widget_type": "collection-field",
  "uuid": "field-123",
  "data": {
    "collection_code": "team",
    "entry_source": "static",
    "entry_id": "entry-uuid-456",
    "field_code": "bio",
    "display_as": "p"
  },
  "settings": {}
}
```

## Example Response (URL-based Entry)

```json
{
  "widget_type": "collection-field",
  "uuid": "field-456",
  "data": {
    "collection_code": "products",
    "entry_source": "url",
    "entry_url_segment": 2,
    "field_code": "description",
    "display_as": "p"
  },
  "settings": {}
}
```

## Entry Source Values

| Value | Description |
|-------|-------------|
| `static` | Use specific `entry_id` |
| `url` | Extract entry ID from URL segment |

### URL Segment Example

For URL `/products/shoes/running-shoe-123`:
- Segment 1: `products`
- Segment 2: `shoes`
- Segment 3: `running-shoe-123`

If `entry_url_segment` is `3`, it will use `running-shoe-123` as the entry identifier.

## Display As Values

| Value | Description |
|-------|-------------|
| `p` | Paragraph |
| `h1` - `h6` | Heading levels |
| `span` | Inline span |
| `image` | Render as image |
| `gallery` | Render as image gallery |

## Usage Example

```javascript
// Render collection field widget
async function renderCollectionField(widget, language, urlSegments, api) {
  const {
    collection_code, entry_source, entry_id, entry_url_segment,
    field_code, display_as
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

  // Get field value
  const fieldValue = entry.data[field_code];
  let content = '';

  // Handle multilingual fields
  if (typeof fieldValue === 'object' && !Array.isArray(fieldValue) && fieldValue !== null) {
    // Check if it's an image object
    if (fieldValue.url) {
      content = fieldValue;
    } else {
      // Multilingual text
      content = fieldValue[language] || fieldValue.en || '';
    }
  } else {
    content = fieldValue || '';
  }

  // Render based on display_as
  switch (display_as) {
    case 'image':
      return renderImage(content, field_code);
    case 'gallery':
      return renderGallery(content);
    case 'h1':
    case 'h2':
    case 'h3':
    case 'h4':
    case 'h5':
    case 'h6':
      return `<${display_as}>${content}</${display_as}>`;
    case 'span':
      return `<span>${content}</span>`;
    default:
      return `<p>${content}</p>`;
  }
}

function renderImage(imageData, alt) {
  if (typeof imageData === 'string') {
    return `<img src="${imageData}" alt="${alt}">`;
  }
  if (imageData?.url) {
    return `<img src="${imageData.url}" alt="${imageData.alt || alt}">`;
  }
  return '';
}

function renderGallery(images) {
  if (!Array.isArray(images)) return '';

  const items = images.map(img => {
    const url = typeof img === 'string' ? img : img.url;
    return `<img src="${url}" alt="" loading="lazy">`;
  }).join('');

  return `<div class="field-gallery">${items}</div>`;
}
```

## Use Cases

- **Dynamic page titles**: Display entry title as h1
- **Product descriptions**: Show product description from collection
- **Dynamic images**: Display entry's featured image
- **SEO content**: Pull meta description from entry fields
- **Template layouts**: Build flexible entry detail pages
