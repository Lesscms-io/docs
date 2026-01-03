# Spacer Widget

An invisible spacing element to add vertical space between widgets.

## Widget Type

```
spacer
```

## Data Properties

| Property | Type | Global | Description |
|----------|------|--------|-------------|
| `height` | number | Yes | Space height in pixels |

## Example Response

```json
{
  "widget_type": "spacer",
  "uuid": "spacer-123",
  "data": {
    "height": 50
  },
  "settings": {}
}
```

## Usage Example

```javascript
// Render spacer widget
function renderSpacer(widget) {
  const { height } = widget.data;

  return `<div style="height: ${height}px;"></div>`;
}
```

## Responsive Considerations

For responsive spacing, you can use the responsive settings override:

```json
{
  "widget_type": "spacer",
  "uuid": "spacer-456",
  "data": {
    "height": 80
  },
  "settings": {
    "responsive": {
      "tablet": {
        "height": 60
      },
      "mobile": {
        "height": 40
      }
    }
  }
}
```
