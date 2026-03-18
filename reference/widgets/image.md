# Image Widget

A single image widget with style presets.

## Widget Type

```
image
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"image"` |
| `uuid` | string | Unique widget identifier |
| `widget` | object | Widget properties |
| `widget.image_source` | string | Content source: `"static"` or `"dynamic"` (default: `"static"`) |
| `widget.image` | string\|null | Image URL |
| `widget.image_style` | string | Image style preset (default: `"none"`) |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "image",
  "uuid": "img-123",
  "widget": {
    "image_source": "static",
    "image": "https://cdn.example.com/images/hero.jpg",
    "image_style": "rounded-shadow"
  },
  "settings": {
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Image Style Presets

| Value | Description | CSS |
|-------|-------------|-----|
| `none` | No style (default) | -- |
| `rounded` | Rounded corners | `border-radius: 12px` |
| `rounded-lg` | Large rounded corners | `border-radius: 24px` |
| `circle` | Circle / ellipse | `border-radius: 50%` |
| `shadow-sm` | Light shadow | `box-shadow: 0 2px 8px rgba(0,0,0,0.1)` |
| `shadow-lg` | Heavy shadow | `box-shadow: 0 8px 24px rgba(0,0,0,0.2)` |
| `rounded-shadow` | Rounded + shadow | `border-radius: 12px; box-shadow: 0 4px 12px rgba(0,0,0,0.15)` |
| `border` | Border | `border: 2px solid #e0e0e0` |
| `border-rounded` | Border + rounded | `border: 2px solid #e0e0e0; border-radius: 12px` |

## Usage Example

```javascript
function renderImage(widget) {
  const { image_source, image, image_style } = widget.widget;

  if (!image) return '<div class="image-placeholder"><i class="fa-solid fa-image"></i></div>';

  const styleMap = {
    'none': '',
    'rounded': 'border-radius: 12px',
    'rounded-lg': 'border-radius: 24px',
    'circle': 'border-radius: 50%',
    'shadow-sm': 'box-shadow: 0 2px 8px rgba(0,0,0,0.1)',
    'shadow-lg': 'box-shadow: 0 8px 24px rgba(0,0,0,0.2)',
    'rounded-shadow': 'border-radius: 12px; box-shadow: 0 4px 12px rgba(0,0,0,0.15)',
    'border': 'border: 2px solid #e0e0e0',
    'border-rounded': 'border: 2px solid #e0e0e0; border-radius: 12px'
  };
  const style = styleMap[image_style] || '';

  return `<img src="${image}" class="img-fluid" style="${style}" />`;
}
```
