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
| `widget` | object | Widget properties |
| `widget.code` | string | HTML/JS embed code |
| `widget.height` | number\|null | Container height in pixels |
| `settings` | object | Style settings (optional) |

## Example Response

```json
{
  "widget_type": "embed",
  "uuid": "embed-123",
  "widget": {
    "code": "<iframe src=\"https://www.youtube.com/embed/dQw4w9WgXcQ\" frameborder=\"0\" allowfullscreen style=\"width:100%;height:100%\"></iframe>",
    "height": 400
  }
}
```

## Script execution

In the official renderer (`@lesscms/vue-widgets`, used by LessCMS-hosted sites) `<script>` tags inside `code` are executed in the browser after the widget mounts:

- in document order; an external `src` script (without `async`) is awaited before the next one runs,
- never during SSR and never inside the panel editor (Page Builder preview),
- non-JavaScript types (e.g. `application/ld+json`) are kept as-is and not executed,
- when `code` changes, pending scripts of the previous version are cancelled (side effects of scripts that already ran are not undone).

If you render `code` yourself with `innerHTML`, browsers do **not** execute scripts — re-create each `<script>` element as shown below.

## Usage Example

```javascript
function renderEmbed(widget) {
  const { code, height } = widget.widget;

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

## Running scripts in your own frontend

```javascript
function mountEmbed(container, code) {
  container.innerHTML = code;
  // innerHTML never runs <script> — re-create them so the browser executes them
  container.querySelectorAll('script').forEach((old) => {
    const s = document.createElement('script');
    for (const { name, value } of old.attributes) s.setAttribute(name, value);
    s.textContent = old.textContent;
    old.replaceWith(s);
  });
}
```
