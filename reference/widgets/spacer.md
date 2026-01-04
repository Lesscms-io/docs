# Spacer Widget

An invisible spacing element to add vertical space between widgets.

## Widget Type

```
spacer
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"spacer"` |
| `uuid` | string | Unique widget identifier |
| `config` | object | Widget configuration |
| `config.height` | number | Space height in pixels |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "spacer",
  "uuid": "spacer-123",
  "config": {
    "height": 50
  },
  "settings": {
    "responsive": {
      "tablet": {},
      "mobile": {}
    }
  }
}
```

## Usage Example

```javascript
function renderSpacer(widget) {
  const { height } = widget.config;

  return `<div class="spacer" style="height: ${height}px;"></div>`;
}
```
