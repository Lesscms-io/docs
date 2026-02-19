# Embed Widget

Custom HTML/JavaScript embed code for third-party integrations.

## Widget Type

```
embed
```

## Response Structure

| Property | Type | Description |
|----------|------|-------------|
| `widget_type` | string | Always `"embed"` |
| `uuid` | string | Unique widget identifier |
| `config` | object | Widget configuration |
| `config.code` | string | HTML/JS embed code |
| `config.height` | number\|null | Container height in pixels |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "embed",
  "uuid": "embed-123",
  "config": {
    "code": "<iframe src=\"https://www.youtube.com/embed/dQw4w9WgXcQ\" frameborder=\"0\" allowfullscreen style=\"width:100%;height:100%\"></iframe>",
    "height": 400
  }
}
```

## Usage Example

```javascript
function renderEmbed(widget) {
  const { code, height } = widget.config;

  if (!code) return '';

  const heightStyle = height ? `height: ${height}px;` : '';

  return `
    <div class="embed-container" style="width: 100%; overflow: hidden; ${heightStyle}">
      ${code}
    </div>
  `;
}
```

## Security Note

Embed code is rendered as raw HTML. When using this widget, ensure you trust the content source. Consider using CSP headers and sanitizing the embed code if displaying user-generated content.
